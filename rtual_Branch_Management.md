import os
import json
import shutil
from datetime import datetime
from pathlib import Path
from typing import List, Dict, Optional
from dataclasses import dataclass, field
import logging

# Logging configuration
logging.basicConfig(level=logging.INFO, format='%(asctime)s - %(levelname)s - %(message)s')

@dataclass
class Branch:
    """Class representing a virtual branch."""
    name: str
    parent: Optional[str] = None
    created_at: str = field(default_factory=lambda: datetime.now().isoformat())
    data_weight: float = 0.0  # Data weight (Tar payload)
    commits: List[str] = field(default_factory=list)
    
    def to_dict(self) -> Dict:
        return {
            'name': self.name,
            'parent': self.parent,
            'created_at': self.created_at,
            'data_weight': self.data_weight,
            'commits': self.commits
        }

class VirtualBranchManager:
    """
    Virtual branch management system with Fork, Tar, and Scissors capabilities.
    """
    
    def __init__(self, repository_path: str):
        """
        Initialize the Virtual Branch Manager.
        
        Args:
            repository_path: Path to the virtual repository directory.
        """
        self.repository = Path(repository_path)
        self.branches_file = self.repository / '.virtual_branches.json'
        self.branches: Dict[str, Branch] = {}
        self.current_branch: str = "main"
        
        self.repository.mkdir(parents=True, exist_ok=True)
        self._load_branches()
        
        if not self.branches:
            self.create_branch("main")
            self.current_branch = "main"
        
        logging.info(f"Virtual Branch Manager initialized at: {self.repository}")
    
    def _load_branches(self):
        """Load branches from the JSON metadata file."""
        if self.branches_file.exists():
            try:
                with open(self.branches_file, 'r', encoding='utf-8') as f:
                    data = json.load(f)
                    for name, branch_data in data.items():
                        if name != '_current_branch':
                            self.branches[name] = Branch(**branch_data)
                    self.current_branch = data.get('_current_branch', 'main')
            except Exception as e:
                logging.error(f"Error loading branches: {e}")
    
    def _save_branches(self):
        """Save branches to the JSON metadata file."""
        try:
            data = {name: branch.to_dict() for name, branch in self.branches.items()}
            data['_current_branch'] = self.current_branch
            with open(self.branches_file, 'w', encoding='utf-8') as f:
                json.dump(data, f, indent=2, ensure_ascii=False)
        except Exception as e:
            logging.error(f"Error saving branches: {e}")
    
    def create_branch(self, name: str, parent: Optional[str] = None) -> bool:
        """
        Create a new virtual branch.
        
        Args:
            name: Name of the new branch.
            parent: Name of the parent branch (optional).
            
        Returns:
            bool: True if successful, False otherwise.
        """
        if name in self.branches:
            logging.error(f"Branch '{name}' already exists.")
            return False
        
        if parent and parent not in self.branches:
            logging.error(f"Parent branch '{parent}' does not exist.")
            return False
        
        self.branches[name] = Branch(name=name, parent=parent)
        self._save_branches()
        logging.info(f"Branch created: {name} (parent: {parent or 'None'})")
        return True
    
    def fork_branch(self, source_name: str, new_name: str) -> bool:
        """
        Create a fork from an existing branch.
        
        Args:
            source_name: Name of the source branch to fork from.
            new_name: Name of the new forked branch.
            
        Returns:
            bool: True if successful, False otherwise.
        """
        if source_name not in self.branches:
            logging.error(f"Source branch '{source_name}' does not exist.")
            return False
        
        if new_name in self.branches:
            logging.error(f"Branch '{new_name}' already exists.")
            return False
        
        source_branch = self.branches[source_name]
        
        # Deep copy data and commits from the source branch
        new_branch = Branch(
            name=new_name,
            parent=source_name,
            data_weight=source_branch.data_weight,
            commits=source_branch.commits.copy()
        )
        
        self.branches[new_name] = new_branch
        self._save_branches()
        logging.info(f"Forked branch: {source_name} -> {new_name}")
        return True
    
    def add_tar_data(self, branch_name: str, weight: float) -> bool:
        """
        Add heavy data payload to a branch (Tar operation).
        
        Args:
            branch_name: Name of the target branch.
            weight: Data weight to add in megabytes.
            
        Returns:
            bool: True if successful, False otherwise.
        """
        if branch_name not in self.branches:
            logging.error(f"Branch '{branch_name}' does not exist.")
            return False
        
        self.branches[branch_name].data_weight += weight
        self._save_branches()
        logging.info(f"Added {weight} MB of tar data to branch: {branch_name}")
        return True
    
    def cut_branch(self, branch_name: str, cut_size: float) -> bool:
        """
        Cut and prune branch data payload (Scissors operation).
        
        Args:
            branch_name: Name of the target branch.
            cut_size: Amount of data to cut in megabytes.
            
        Returns:
            bool: True if successful, False otherwise.
        """
        if branch_name not in self.branches:
            logging.error(f"Branch '{branch_name}' does not exist.")
            return False
        
        branch = self.branches[branch_name]
        
        if cut_size > branch.data_weight:
            logging.warning(f"Cut size ({cut_size} MB) exceeds branch weight ({branch.data_weight} MB). Trimming to max.")
            cut_size = branch.data_weight
        
        branch.data_weight -= cut_size
        self._save_branches()
        logging.info(f"Cut {cut_size} MB from branch: {branch_name}")
        return True
    
    def switch_branch(self, branch_name: str) -> bool:
        """
        Switch the currently active virtual branch.
        
        Args:
            branch_name: Name of the branch to switch to.
            
        Returns:
            bool: True if successful, False otherwise.
        """
        if branch_name not in self.branches:
            logging.error(f"Branch '{branch_name}' does not exist.")
            return False
        
        self.current_branch = branch_name
        self._save_branches()
        logging.info(f"Switched to branch: {branch_name}")
        return True
    
    def commit_to_branch(self, branch_name: str, message: str) -> bool:
        """
        Record a commit message in the specified branch.
        
        Args:
            branch_name: Name of the target branch.
            message: The commit message.
            
        Returns:
            bool: True if successful, False otherwise.
        """
        if branch_name not in self.branches:
            logging.error(f"Branch '{branch_name}' does not exist.")
            return False
        
        commit_id = datetime.now().strftime('%Y%m%d_%H%M%S')
        self.branches[branch_name].commits.append(f"{commit_id}: {message}")
        self._save_branches()
        logging.info(f"Commit added to {branch_name}: {message}")
        return True
    
    def list_branches(self) -> List[Dict]:
        """
        List all virtual branches and their metadata.
        
        Returns:
            List[Dict]: A list of dictionaries containing branch information.
        """
        return [
            {
                'name': branch.name,
                'parent': branch.parent,
                'created_at': branch.created_at,
                'data_weight_mb': round(branch.data_weight, 2),
                'commits_count': len(branch.commits),
                'is_current': branch.name == self.current_branch
            }
            for branch in self.branches.values()
        ]
    
    def delete_branch(self, branch_name: str) -> bool:
        """
        Delete a virtual branch.
        
        Args:
            branch_name: Name of the branch to delete.
            
        Returns:
            bool: True if successful, False otherwise.
        """
        if branch_name not in self.branches:
            logging.error(f"Branch '{branch_name}' does not exist.")
            return False
        
        if branch_name == "main":
            logging.error("Cannot delete the main branch.")
            return False
        
        if branch_name == self.current_branch:
            logging.error("Cannot delete the currently active branch.")
            return False
        
        del self.branches[branch_name]
        self._save_branches()
        logging.info(f"Branch deleted: {branch_name}")
        return True
    
    def get_branch_tree(self) -> Dict:
        """
        Generate a hierarchical tree structure of all branches.
        
        Returns:
            Dict: A dictionary representing the parent-child relationships.
        """
        tree = {}
        for branch in self.branches.values():
            if branch.parent:
                if branch.parent not in tree:
                    tree[branch.parent] = []
                tree[branch.parent].append(branch.name)
            else:
                if branch.name not in tree:
                    tree[branch.name] = []
        
        return tree


# Example usage
if __name__ == "__main__":
    # Initialize the manager
    manager = VirtualBranchManager("./my_virtual_repo")
    
    # Create branches
    manager.create_branch("feature-1", parent="main")
    manager.create_branch("feature-2", parent="main")
    
    # Fork a branch
    manager.fork_branch("feature-1", "feature-1-fork")
    
    # Add Tar (heavy data payload)
    manager.add_tar_data("feature-1", 150.5)
    manager.add_tar_data("feature-1-fork", 75.3)
    
    # Scissors (cut/prune data)
    manager.cut_branch("feature-1", 50.0)
    
    # Commit changes
    manager.commit_to_branch("feature-1", "Added new virtual feature")
    
    # List all branches
    branches = manager.list_branches()
    print("\n--- Branch List ---")
    for branch in branches:
        current_marker = " *" if branch['is_current'] else ""
        print(f"{branch['name']}{current_marker} (Weight: {branch['data_weight_mb']} MB, Commits: {branch['commits_count']})")
    
    # Display branch tree
    tree = manager.get_branch_tree()
    print("\n--- Branch Tree ---")
    for parent, children in tree.items():
        print(f"{parent} -> {children}")
