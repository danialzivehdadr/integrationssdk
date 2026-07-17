# DEXTools Comprehensive Integration Guide

> **Version:** 1.1.0  
> **Scope:** Frontend Widget, Token Metadata API, and Blockchain/DEX Protocol Integration  
> **Audience:** Frontend Developers, Backend Engineers, and Blockchain Protocol Teams  

Welcome to the official DEXTools integration guide. This document provides all necessary instructions for embedding the DEXTswap Aggregator Widget, managing token metadata via our Partner API, and integrating entire blockchains or DEX protocols into the DEXTools ecosystem.

---

## 📌 Table of Contents
1. [Part 1: Frontend Integration (DEXTswap Widget)](#part-1-frontend-integration-dextswap-widget)
2. [Part 2: Backend API (Token Socials & Media)](#part-2-backend-api-token-socials--media)
3. [Part 3: Protocol API (DEX/Blockchain Integration)](#part-3-protocol-api-dexblockchain-integration)

---

## Part 1: Frontend Integration (DEXTswap Widget)

### Overview & Quick Start
The **DEXTswap Aggregator Widget** allows websites to seamlessly embed the DEXTswap token swap aggregator, supporting multiple blockchains with the same experience as the main [DEXTools.io](https://www.dextools.io) app.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>DEXTswap Aggregator Widget</title>
  <style>
    .dextswap-widget { border: none; border-radius: 8px; max-width: 100%; box-shadow: 0 4px 6px rgba(0,0,0,0.1); }
  </style>
</head>
<body>
  <iframe 
    id="dextswap-aggregator-widget"
    class="dextswap-widget"
    title="DEXTswap Aggregator"
    width="400" 
    height="420" 
    src="https://www.dextools.io/widget-aggregator/en/swap/eth/0x6982508145454ce325ddbe47a25d4ec3d2311933?theme=dark">
  </iframe>
</body>
</html>
```

### Configuration & Customization
Base URL structure: `https://www.dextools.io/widget-aggregator/en/swap/<chainId>/<tokenAddress>?theme=<theme>`

| Parameter | Type | Required | Description |
| :--- | :--- | :---: | :--- |
| `chainId` | String | **Yes** | Target blockchain ID (e.g., `eth`, `bnb`). |
| `tokenAddress` | String | **Yes** | Contract address of the `token-in`. `token-out` defaults to the chain's native token. |
| `theme` | String | No | UI theme: `dark` (default) or `light`. |

### Supported Blockchains
| Blockchain | `chainId` | Blockchain | `chainId` |
| :--- | :--- | :--- | :--- |
| Ethereum | `eth` | Arbitrum | `arbitrum` |
| BNB Chain | `bnb` | Avalanche | `avalanche` |
| Polygon | `polygon` | Optimism | `optimism` |
| Cronos | `cronos` | zkSync | `zksync` |
| OKTC | `oktc` | Base | `base` |
| Solana | `solana` | Sonic | `sonic` |

### ⚠️ Deployment Rules
- 🚫 **No Localhost:** The widget will **NOT** work on `localhost`. Use a live domain (e.g., `https://yourdomain.com`).
- 🔒 **CSP Headers:** Ensure your server allows iframe embedding from `https://www.dextools.io`.

---

## Part 2: Backend API (Token Socials & Media)

### Authentication
This API is exclusively for official DEXTools partners. Contact [marketing@dextools.io](mailto:marketing@dextools.io) for an API key.  
All requests require:
```http
X-API-Key: YOUR_API_KEY
Content-Type: application/json
```

### Endpoint 1: Update Token Socials
**`POST /v1/token`**  
Creates or updates social media profiles, media assets, and repository links for a token.

#### Request Body
| Field | Type | Required | Description & Constraints |
| :--- | :--- | :---: | :--- |
| `chain` | String | **Yes** | Blockchain identifier (e.g., `"eth"`). |
| `address` | String | **Yes** | Contract address of the token. |
| `creationTransactionHash`| String | No | Transaction hash of token creation *(EVM chains only)*. |
| `socials` | Object | No | Parent object for social/media data. |
| `socials.description` | String | No | Brief description of the token. |
| `socials.email` | String | No | Official contact email. |
| `socials.logo` | String | No | Base64 or URL. ⚠️ **Max:** 250x250 px, 200 KB. |
| `socials.banner` | String | No | Base64 or URL. ⚠️ **Max:** 600x200 px, 1024 KB. |
| `socials.twitter` | String | No | Official X (Twitter) profile URL. |
| `socials.telegram` | String | No | Official Telegram channel/group URL. |
| `socials.discord` | String | No | Official Discord server URL. |
| `socials.facebook` | String | No | Official Facebook page URL. |
| `socials.youtube` | String | No | Official YouTube channel URL. |
| `socials.instagram` | String | No | Official Instagram profile URL. |
| `socials.reddit` | String | No | Official Reddit community URL. |
| `socials.tiktok` | String | No | Official TikTok profile URL. |
| `socials.website` | Array | No | Array of official website URLs. |
| `socials.repos.github` | Array | No | Array of GitHub repository URLs. |
| `socials.repos.bitbucket`| Array | No | Array of Bitbucket repository URLs. |

#### Example Request
```json
{
  "chain": "eth",
  "address": "0x6982508145454ce325ddbe47a25d4ec3d2311933",
  "creationTransactionHash": "0x2afae7763487e60b893cb57803694810e6d3d136186a6de6719921afd7ca304a",
  "socials": {
    "description": "Official token project description.",
    "email": "contact@example.com",
    "logo": "https://example.com/logo.png",
    "banner": "https://example.com/banner.png",
    "twitter": "https://twitter.com/example",
    "telegram": "https://t.me/example",
    "website": ["https://example.com"],
    "repos": { "github": ["https://github.com/example/repo"], "bitbucket": [] }
  }
}
```

### Endpoint 2: Check Token Socials Status
**`GET /v1/token/:chain/:address`**  
Verifies if a token already has social information populated.

#### Path Parameters
| Parameter | Type | Required | Description |
| :--- | :--- | :---: | :--- |
| `chain` | String | **Yes** | Blockchain identifier (e.g., `"eth"`). |
| `address` | String | **Yes** | Contract address of the token. |

#### Example Response
```json
{
  "statusCode": 200,
  "data": { "existingSocialsInfo": true }
}
```

### API Response Codes
| HTTP Status | Description |
| :--- | :--- |
| **200 OK** | Request successful (data created/updated or status returned). |
| **400 Bad Request** | Invalid payload, missing required fields, or image limits exceeded. |
| **403 Forbidden** | Missing, invalid, or unauthorized `X-API-Key`. |

---

## Part 3: Protocol API (DEX/Blockchain Integration)

### Overview & Integration Models
This HTTP API specification is required for any blockchain or DEX to be indexed by DEXTools. DEXTools will consume this API to index all trading data, requesting each block as fast as possible.

- **Blockchain-Level Integration:** A single API server provides data for multiple DEXs on the same chain (e.g., `dextools-api.mychain.com`). The `/exchange` endpoint **must** be implemented.
- **DEX-Level Integration:** Each individual DEX provides its own dedicated API URL (e.g., `dextools-api.one-dex.com`).

### Core Endpoints Summary
| Method | Path | Description |
| :--- | :--- | :--- |
| `GET` | `/latest-block` | Returns the latest fully processed block. |
| `GET` | `/block` | Returns a specific block by `number` or `timestamp`. |
| `GET` | `/asset` | Returns token metadata by contract address (`id`). |
| `GET` | `/asset/holders` | Returns a paginated, descending-sorted list of token holders. |
| `GET` | `/exchange` | Returns DEX details by factory address or ID. |
| `GET` | `/pair` | Returns liquidity pair (pool) details by address (`id`). |
| `GET` | `/events` | Returns a list of events (`creation`, `swap`, `join`, `exit`) within a block range (`fromBlock`, `toBlock`). |

### Key Data Models (Schemas)

#### Event Schema (Core Indexing)
```json
{
  "block": { "blockNumber": 18500000, "blockTimestamp": 1698765432 },
  "txnId": "0x9876...dcba",
  "txnIndex": 45,
  "eventIndex": 2,
  "maker": "0x1234...abcd",
  "pairId": "0xB4e16d0168e52d35CaCD2c6185b44281Ec28C9Dc",
  "eventType": "swap",
  "asset0In": "1000000",
  "asset1Out": "0.5",
  "reserves": { "asset0": "50000000", "asset1": "25" }
}
```
*Note: `eventType` must be `creation`, `swap`, `join`, or `exit`. Conditional fields like `amount0`, `asset0In`, etc., apply based on the event type.*

#### Pair Schema
```json
{
  "id": "0xB4e16d0168e52d35CaCD2c6185b44281Ec28C9Dc",
  "asset0Id": "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
  "asset1Id": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
  "createdAtBlockNumber": 10000835,
  "createdAtBlockTimestamp": 1588710145,
  "createdAtTxnId": "0x1234...abcd",
  "factoryAddress": "0x5C69bEe701ef814a2B6a3EDD4B1652CB9cc5aA6f"
}
```

### Standardized Error Handling
All endpoints may return standard HTTP error codes. For `400`, `404`, `429`, and `500` errors, the response body will follow this schema:

```json
{
  "code": "INVALID_PARAMETER",
  "message": "The provided block number is invalid.",
  "issues": [
    {
      "code": "OUT_OF_RANGE",
      "param": "fromBlock",
      "message": "Block number must be greater than 0."
    }
  ]
}
```

| Status Code | Description |
| :--- | :--- |
| **400 Bad Request** | Invalid payload, missing required query parameters, or malformed data. |
| **404 Not Found** | The requested resource (block, asset, pair, or exchange) does not exist. |
| **429 Too Many Requests** | Rate limit exceeded. Implement exponential backoff. |
| **500 Internal Server Error** | An unexpected error occurred on the API server. |

---
> *Consistency Note: Ensure the `chain` identifier used in the Backend API exactly matches the `chainId` used in the Frontend Widget to guarantee seamless data correlation across your integration.*
