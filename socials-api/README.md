
---

## 📡 Endpoints

### 1. Update Token Social Information
**`POST /v1/token`**

Creates or updates the social media profiles, media assets, and repository links associated with a specific token.

#### Request Body Parameters
| Field | Type | Required | Description & Constraints |
| :--- | :--- | :---: | :--- |
| `chain` | String | **Yes** | The blockchain identifier (e.g., `"eth"`, `"bnb"`). |
| `address` | String | **Yes** | The contract address of the token to update. |
| `creationTransactionHash` | String | No | The transaction hash of the token's creation. *(Note: Only applicable and processed for **EVM** chains)*. |
| `socials` | Object | No | Parent object containing all social, media, and repository data. |
| `socials.description` | String | No | A brief description of the token to be displayed on DEXTools. |
| `socials.email` | String | No | Official contact email address. |
| `socials.logo` | String | No | Logo image as a Base64 string or public URL. <br>⚠️ **Max Dimensions:** 250x250 px \| **Max File Size:** 200 KB |
| `socials.banner` | String | No | Banner image as a Base64 string or public URL. <br>⚠️ **Max Dimensions:** 600x200 px \| **Max File Size:** 1024 KB |
| `socials.twitter` | String | No | Official X (Twitter) profile URL. |
| `socials.telegram` | String | No | Official Telegram channel or group URL. |
| `socials.discord` | String | No | Official Discord server invite URL. |
| `socials.facebook` | String | No | Official Facebook page URL. |
| `socials.youtube` | String | No | Official YouTube channel URL. |
| `socials.instagram` | String | No | Official Instagram profile URL. |
| `socials.reddit` | String | No | Official Reddit community URL. |
| `socials.tiktok` | String | No | Official TikTok profile URL. |
| `socials.website` | Array | No | Array of official website URLs (e.g., `["https://example.com"]`). |
| `socials.repos` | Object | No | Parent object for source code repository information. |
| `socials.repos.github` | Array | No | Array of GitHub repository URLs. |
| `socials.repos.bitbucket`| Array | No | Array of Bitbucket repository URLs. |

#### Example Request
```json
{
  "chain": "eth",
  "address": "0x6982508145454ce325ddbe47a25d4ec3d2311933",
  "creationTransactionHash": "0x2afae7763487e60b893cb57803694810e6d3d136186a6de6719921afd7ca304a",
  "socials": {
    "description": "Official description of the token project.",
    "email": "contact@example.com",
    "logo": "https://example.com/assets/logo.png",
    "banner": "https://example.com/assets/banner.png",
    "twitter": "https://twitter.com/example",
    "telegram": "https://t.me/example",
    "website": [ "https://example.com" ],
    "repos": {
      "github": [ "https://github.com/example/repo" ],
      "bitbucket": []
    }
  }
}
