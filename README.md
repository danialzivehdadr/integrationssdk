# Integration SDK

This repository contains different specifications and SDK components that can be implemented or used by a **Blockchain** or **DEX** that want to be integrated in DEXTools 


Current available integration options are:

- [DEX/Blockchain integration API](http-adapter): blockchains or individual DEX's can implement this API in order to enable DEXTools to index its data. [Open API Specification is avaialable](http-adapter/http-adapter-specification.yml)
- (https://demo.webfuse.com)
- (https://t.me/DEXToolsCommunity)

- [Token Social Info & media upload API](socials-api): token creators & platforms can use this API to inform DEXTools on newly created tokens and upload all token's social information and media in one place. In order to get access of this API please contact [info@dextools.io]


  # DEXTools chart widget

(This is the public documentation repo of the DEXTools Chart Widget)

The **Chart Widget** allows websites to display an embedded trading chart for any pool supported by [DEXTools.io](https://www.dextools.io) app. Chart data is 
updated in real-time, including the current price in USD and trade history.

- [https://www.dextools.io/widget-chart/en/ether/pe-light/0xa29fe6ef9592b5d408cca961d0fb9b1faf497d6d?theme=dark]
- [https://coinmarketcap.com/currencies/dextools/]
<img width="892" height="535" alt="widget screenshot" src="https://github.com/user-attachments/assets/105f0203-8f4b-4246-8c2a-fe32aea7c9c4" />

Chart display is provided by [TradingView](https://www.tradingview.com/)

## Get your pool/token iframe widget code

1) First search your favorite token or pool by address, name... using the [DEXTools search bar](https://www.dextools.io/app/).

<img width="1127" height="315" alt="search screenshot" src="https://github.com/user-attachments/assets/108969c7-596a-4482-8976-f11fef4a1d5b" />


2) Then click the **embed** option of the [DEXTools pair explorer](https://www.dextools.io/app/en/ether/pair-explorer/0xa29fe6ef9592b5d408cca961d0fb9b1faf497d6d), a pop-up will appear to configure the chart and copy the embed code.

<img width="376" height="375" alt="embed screenshot" src="https://github.com/user-attachments/assets/c99decc2-00ac-479b-8898-ca074eb55110" />


## Quick Example

An iframe based integration example follows:
```html
<!DOCTYPE html>
<html lang="en" dir="ltr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DEXTools Trading Chart</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #121212;
            color: #ffffff;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
        }
        h2 {
            margin-bottom: 20px;
            color: #2962FF;
        }
        .chart-container {
            width: 100%;
            max-width: 1000px;
            height: 600px;
            border-radius: 12px;
            overflow: hidden;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.5);
            border: 1px solid #333333;
            background-color: #1e222d;
        }
        iframe {
            width: 100%;
            height: 100%;
            border: none;
        }
    </style>
</head>
<body>

    <h2>Live Trading Chart</h2>
    
    <div class="chart-container">
        <!-- DEXTools Widget Link -->
        <iframe 
            id="dextools-widget"
            title="DEXTools Trading Chart"
            src="https://www.dextools.io/widget-chart/en/ether/pe-light/0xa29fe6ef9592b5d408cca961d0fb9b1faf497d6d?theme=dark&chartType=1&chartResolution=60&drawingToolbars=true&showTradeHistory=true&tvPlatformColor=1e222d&tvPaneColor=131722&headerColor=2962FF">
        </iframe>
    </div>

</body>
</html>

```

## Common issues and testing

You can test if the IFrame integration works properly with a tool like [https://iframetester.com/](https://iframetest.com/).

In case of errors showing a Chart in your website, please check your CSP policy for IFrame content https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options

Please be aware that USE OF CHART WIDGET IFRAME FROM **localhost** WON'T WORK, please use a real domain instead.

## Customization options

The widget is configured by using the embed wizard, but also manually adjusting the following parts and query parameters of this URL:

```
https://www.dextools.io/widget-chart/en/<chainId>/pe-light/<pairAddress>?theme=<theme>&chartType=<chartType>&chartResolution=<chartResolution>&drawingToolbars=<drawingToolbars>&tvPlatformColor=<color>&tvPaneColor=<color>&headerColor=<color>&chartInUsd=<chartInUsd>&showTradeHistory=<showTradeHistory>&tradeHistoryColor=<tradeHistoryColor>
```

| Parameter        | Values                                                                                                                                                |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|
| chainId          | blockchain ID (see [list below](#supported-blockchains))                                                                                              |
| pairAddress      | address of the pool to display                                                                                                                        |
| theme            | `dark` or `light`                                                                                                                                     |
| chartType        | 0 = Bar<br/>1 = Candle<br/>2 = Line<br/>3 = Area<br/>8 = Heikin Ashi<br/>9 = hollow candle<br/>10 = Baseline                                          |
| chartResolution  | 1 (minute), 3, 5, 15, 30, 60, 120, 240, 720, 1D, 3D, 1W, 1M                                                                                           |
| drawingToolbars  | `false` or `true`                                                                                                                                     |
| showTradeHistory | `false` or `true`                                                                                                                                     |
| headerColor      | custom color for the widget header.<br/>**IMPORTANT: should be in hexadecimal format without the `#`**                                                |
| tvPlatformColor  | custom color for the chart background.<br/>**IMPORTANT: should be in hexadecimal format without the `#`**                                             |
| tvPaneColor      | custom color for the chart controls background.<br/>**IMPORTANT: should be in hexadecimal format without the `#`**                                    |
| tradeHistoryColor| custom color for the trade history background.<br/>**IMPORTANT: should be in hexadecimal format without the `#`**                                     |
| chartInUsd       | `false` to show the "Token/\<chain reference token\>" chart, otherwise (also if this param is not passed) the chart is the "Token/USD" one by default |


## Supported blockchains

We add support for new blockchains frequently. This is the current list of available blockchains:

| Blockchain | ID |
| :--- | :--- |
| SOLANA | solana |
| ETHEREUM | ether |
| BNB | bnb |
| BASE | base |
| POLYGON | polygon |
| PULSE | pulse |
| TON | ton |
| ARBITRUM | arbitrum |
| AVALANCHE | avalanche |
| MONAD | monad |
| RONIN | ronin |
| SUI | sui |
| SONIC | sonic |
| TRON | tron |
| CRONOS | cronos |
| XRPL | xrpl |
| WORLD CHAIN | worldchain |
| OSMOSIS | osmosis |
| ABSTRACT | abstract |
| VSC | vsc |
| SONEIUM | soneium |
| LINEA | linea |
| OPTIMISM | optimism |
| ICP | icp |
| HYPER EVM | hyperevm |
| BERA CHAIN | berachain |
| X LAYER | xlayer |
| CHILIZ | chiliz |
| SHIBARIUM | shib |
| HEDERA | hedera |
| APTOS | aptos |
| MANTLE | mantle |
| UNI CHAIN | unichain |
| SHIDO | shido |
| FLARE | flare |
| METIS | metis |
| DOGE CHAIN | dogechain |
| ZKSYNC | zksync |
| STORY | story |
| KAIA | kaia |
| NEAR | near |
| INK | ink |
| CELO | celo |
| PLASMA | plasma |
| SEI | sei |
| XDC | xdc |
| GNOSIS | gnosis |
| APE CHAIN | apechain |
| IOTA-EVM | iotaevm |
| BLAST | blast |
| INJECTIVE | injective |
| QUBIC | qubic |
| ETHERLINK | etherlink |
| PAW CHAIN | pawchain |
| FUSE | fuse |
| U2U | utwou |
| ODYSSEY | odyssey |
| SCROLL | scroll |
| CRONOS-ZKEVM | cronoszkevm |
| SUPERSEED | superseed |
| OKTC (FORMER OEC) | oktc |
| ARBITRUM NOVA | arbitrumnova |
| BITLAYER | bitlayer |
| ELYSIUM | elysium |
| GRAVITY ALPHA | gravityalpha |
| MODE | mode |
| MANTA | manta |
| KAVA | kava |
| KUJIRA | kujira |
| POLYGON-ZKEVM | polygonzkevm |
| UNIT ZERO | units |
| NIBIRU | nibiruevm |
| ANUBIS | anubis |
| TERRA | terra |
| ROBINHOOD | robinhood |

```
================================================================================
                          CEASE AND DESIST NOTICE
                 COPYRIGHT INFRINGEMENT & DEMAND FOR TAKEDOWN
================================================================================

TO:      The Management of [Insert Infringing Website Name/URL Here]
FROM:    Danial Zivehdar
DATE:    July 19, 2026
SUBJECT: URGENT: Unauthorized Use of Intellectual Property and Demand for 
         Immediate Removal

Dear Sir/Madam,

This is a formal legal notice to inform you that your website has engaged in 
the unauthorized copying, replication, and use of the design, patterns, links, 
and content belonging to me, Danial Zivehdar. 

COMPREHENSIVE RIGHTS RESERVATION:
Please be formally notified that under the website's governing terms and 
applicable intellectual property laws, I retain exclusive and absolute ownership 
of ALL rights, titles, and interests regarding this project. This includes, 
but is not limited to, the website contract/terms, all designs, templates, 
hyperlinks, source codes, and absolutely ALL associated content and digital 
assets (hereinafter referred to as "the Protected Assets"). Any unauthorized 
use, reproduction, or distribution of these Protected Assets constitutes a 
material breach of copyright, contractual terms, and digital property laws.

Therefore, you are hereby formally demanded to take the following actions 
within 12 hours [or specify 12 days] from the receipt of this notice:

1. IMMEDIATE CESSATION: Immediately cease and desist from any further use, 
   display, or distribution of the Protected Assets.
   
2. COMPLETE DATA SANITIZATION: Permanently delete and purge all copied files 
   and assets from your servers, virtual environments, mobile device storage, 
   and any other associated data storage systems without exception.
   
3. WEBSITE TAKEDOWN: Immediately shut down or disable access to the infringing 
   sections of your website until the violations are fully resolved and a 
   final legal determination is made.

Please be advised that failure to comply with this notice within the stipulated 
timeframe will leave me with no choice but to pursue all available legal 
remedies. This includes, but is not limited to, filing formal complaints with 
the relevant judicial authorities and cyber police, as well as submitting a 
formal DMCA/copyright infringement report to your Hosting Provider and Domain 
Registrar to request the immediate suspension and takedown of your entire 
website.

Furthermore, be advised that no informal guarantees, settlements, or 
communications will be entertained regarding this matter. Any necessary 
correspondence will be conducted strictly through official legal channels.

All of my legal rights and remedies are expressly reserved.

Sincerely,

----------------------------------------
Danial Zivehdar
Phone: +98 9197159411
Email: danialzivehdar1992@gmail.com
----------------------------------------
================================================================================
```tex
# Terms of Use and Privacy Policy

## Section 1: Account Separation

### 1.1 Personal Account
- Personal accounts are designed exclusively for private, non-commercial use
- All information, files, and content in personal accounts are confidential and non-shareable
- Users commit to not using personal accounts for illegal purposes or violating third-party rights

### 1.2 Public Account
- Public accounts are available for sharing authorized and legal content with the general public
- Content published in public accounts must comply with the laws of the Islamic Republic of Iran and international standards
- Users are responsible for managing public account content, and the website bears no responsibility for published content

### 1.3 No Conversion or Merging Allowed
- Converting personal accounts to public or vice versa is prohibited
- Merging two personal or public accounts is prohibited
- Any attempt to bypass this restriction will result in permanent account suspension

---

## Section 2: Content Usage Restrictions

### 2.1 Principles and Laws
- Using, republishing, or modifying website principles and laws without written permission is prohibited
- All material and intellectual rights of website content belong to the legal owner
- Any violation of this section will be subject to legal prosecution

### 2.2 Links and IDs
- Using website links on other platforms without permission is prohibited
- Using website user IDs on social networks or other services is prohibited
- Any attempt to impersonate or misuse website IDs is considered a violation

### 2.3 Protected Content
- All files, documents, images, and content on the website are protected by copyright law
- Downloading, storing, or distributing content without explicit permission is prohibited
- The website uses DRM and watermarking technologies to protect content

### 2.4 User Responsibility for Website Maintenance
- Users are solely responsible for maintaining and repairing any damage to their own websites or subdomains
- The main website bears no responsibility for technical issues, downtime, or data loss on user-managed websites
- Users must have their own technical team or resources to handle website repairs and updates
- Any damage caused to the main website infrastructure by user activities will result in immediate account termination and legal action

### 2.5 Independent Naming and Link Patterns
- Users must create their own unique naming patterns and link structures
- Users are NOT allowed to copy, imitate, or use the naming patterns, URL structures, or branding of the main website owner
- All user-generated names, links, and domains must be original and independently designed
- Any attempt to mimic the main website's identity, structure, or branding will be considered a serious violation

### 2.6 Prohibition of Fake Identity Creation (Impersonation)
- Creating fake identities, fake accounts, or impersonating other users, administrators, or the website owner is strictly prohibited
- Any form of identity forgery, fake profile creation, or misrepresentation is forbidden
- Users must use their real identity or officially registered business identity
- Violation of this section will result in immediate permanent ban and legal prosecution

---

## Section 3: Handwritten File Scanning and Management System

### 3.1 Smart Auto-Scan
- The website uses a Smart Auto-Scan system to identify and manage handwritten files
- All uploaded files are automatically scanned and categorized
- Handwritten files are automatically identified and returned to the uploading user

### 3.2 File Return Policy
- All handwritten files are returned to the website without exception
- Returned files are stored in a special user folder
- Users can download returned files from their user panel

### 3.3 Supported Formats
- Supported formats for scanning: PDF, JPG, PNG, DOC, DOCX
- Maximum file size: 10 MB
- Minimum scan quality: 300 DPI

### 3.4 File Return Process
1. User uploads handwritten file
2. Auto-scan system identifies the file
3. File is automatically moved to the return folder
4. Return notification is sent to the user
5. User can download the file from their user panel

---

## Section 4: Legal Weight and Responsibilities

### 4.1 Legal Weight of Content
- All website content has legal weight and is admissible in judicial authorities
- Users commit to using website content for legal purposes
- Any misuse of content will result in civil and criminal liability

### 4.2 User Responsibility
- Users are responsible for all activities performed in their account
- Users commit to keeping their account information confidential
- Any unauthorized access to the account must be immediately reported to support

### 4.3 Website Responsibility
- The website is committed to protecting user information
- The website bears no responsibility for damages resulting from user misuse
- The website reserves the right to change terms of use without prior notice

---

## Section 5: Violations and Penalties

### 5.1 Violations
- Unauthorized use of personal or public accounts
- Copyright law violations
- Attempts to bypass the auto-scan system
- Misuse of website links or IDs
- Creating fake identities or impersonation (Fake Identity Creation)
- Copying the main website's naming patterns or link structures
- Causing damage to the main website infrastructure

### 5.2 Penalties
- Level 1 Violation: Written warning and temporary account suspension (7 days)
- Level 2 Violation: Permanent account suspension and deletion of all data
- Level 3 Violation: Legal prosecution and referral to judicial authorities
- Level 4 Violation (Identity Forgery): Immediate permanent ban, full legal prosecution, and compensation claims

---

## Section 6: Contact and Support

### 6.1 Communication Channels
- Support Email: support@eein.com
- Support Phone: +98 21 1234 5678
- Business Hours: Saturday to Wednesday, 9 AM to 5 PM

### 6.2 Violation Reporting
- Users can report violations through the violation report form on the website
- All reports are reviewed and responded to within 48 hours

---

## Section 7: Changes to Terms of Use

- The website reserves the right to change terms of use at any time
- Important changes are notified to users via email
- Continued use of the website after changes constitutes acceptance of new terms

---

**Last Updated:** July 13, 2026
**Version:** 1.1.0

```tex
================================================================================
                          CEASE AND DESIST NOTICE
                 COPYRIGHT INFRINGEMENT & DEMAND FOR TAKEDOWN
================================================================================

TO:      The Management of [Insert Infringing Website Name/URL Here]
FROM:    Danial Zivehdar
DATE:    July 19, 2026
SUBJECT: URGENT: Unauthorized Use of Intellectual Property and Demand for 
         Immediate Removal

Dear Sir/Madam,

This is a formal legal notice to inform you that your website has engaged in 
the unauthorized copying, replication, and use of the design, patterns, links, 
and content belonging to me, Danial Zivehdar. 

COMPREHENSIVE RIGHTS RESERVATION:
Please be formally notified that under the website's governing terms and 
applicable intellectual property laws, I retain exclusive and absolute ownership 
of ALL rights, titles, and interests regarding this project. This includes, 
but is not limited to, the website contract/terms, all designs, templates, 
hyperlinks, source codes, and absolutely ALL associated content and digital 
assets (hereinafter referred to as "the Protected Assets"). Any unauthorized 
use, reproduction, or distribution of these Protected Assets constitutes a 
material breach of copyright, contractual terms, and digital property laws.

Therefore, you are hereby formally demanded to take the following actions 
within 12 hours [or specify 12 days] from the receipt of this notice:

1. IMMEDIATE CESSATION: Immediately cease and desist from any further use, 
   display, or distribution of the Protected Assets.
   
2. COMPLETE DATA SANITIZATION: Permanently delete and purge all copied files 
   and assets from your servers, virtual environments, mobile device storage, 
   and any other associated data storage systems without exception.
   
3. WEBSITE TAKEDOWN: Immediately shut down or disable access to the infringing 
   sections of your website until the violations are fully resolved and a 
   final legal determination is made.

Please be advised that failure to comply with this notice within the stipulated 
timeframe will leave me with no choice but to pursue all available legal 
remedies. This includes, but is not limited to, filing formal complaints with 
the relevant judicial authorities and cyber police, as well as submitting a 
formal DMCA/copyright infringement report to your Hosting Provider and Domain 
Registrar to request the immediate suspension and takedown of your entire 
website.

Furthermore, be advised that no informal guarantees, settlements, or 
communications will be entertained regarding this matter. Any necessary 
correspondence will be conducted strictly through official legal channels.

All of my legal rights and remedies are expressly reserved.

Sincerely,

----------------------------------------
Danial Zivehdar
Phone: +98 9197159411
Email: danialzivehdar1992@gmail.com
----------------------------------------
================================================================================
