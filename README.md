## SVExtensions Segment Menu
**Customer Segment Menu Visibility for Adobe Commerce**

Version: 1.0.0
Vendor: SVExtensions
Author: Satish Vishwakarma

Table of Contents

1. Overview
2. Features
3. Requirements
4. Installation
5. Configuration
6. Manual Segment Recalculation
7. How It Works
8. Compatibility
9. Support
10. Changelog

> Dynamically hide or show storefront navigation categories based on Adobe Commerce Customer Segments, with full-page cache and Redis compatibility.

---     

## Overview

**SVExtensions_SegmentMenu** gives store administrators granular control over which categories appear in the storefront top navigation menu, based on the customer segment a shopper belongs to.

When a category is restricted to one or more segments, it is automatically hidden from customers who do not belong to those segments — including guest users. The module integrates with Magento's HTTP Context system to ensure correct cache variation per segment, making it fully compatible with Full Page Cache (FPC) and Redis.

---

## Features

- Hide / show categories in top navigation based on customer segment membership
- Guest customer restriction support — guests never see segment-restricted categories
- HTTP Context cache variation — correct per-segment rendering with FPC and Redis
- Admin configuration panel under **Stores → Configuration → Customers → Customer Segment Menu**
- Manual segment recalculation button in admin config — for urgent use or testing without waiting for cron
- Debug logging toggle for troubleshooting segment resolution
- Multi-store / multi-website support
- Compatible with Adobe Commerce B2B environments

---

## Requirements

| Requirement | Version |
|---|---|
| Adobe Commerce | 2.4.x |
| Adobe Commerce B2B | Any compatible version |
| PHP | 8.1 or higher |
| `Magento_CustomerSegment` | Included with Adobe Commerce |

> **Note:** This module requires Adobe Commerce (not Magento Open Source) because it depends on `Magento_CustomerSegment`, which is an Adobe Commerce-only module.

---

## Installation

Install via Composer:

```bash
composer require svextensions/module-segment-menu
```

Enable and deploy:

```bash
bin/magento module:enable SVExtensions_SegmentMenu
bin/magento setup:upgrade
bin/magento setup:di:compile
bin/magento cache:flush
```

---

## Configuration

### Category-level restriction

1. Navigate to **Admin → Catalog → Categories**
2. Select the category you want to restrict
3. Locate the **Allowed Customer Segments** attribute
4. Select one or more customer segments allowed to view this category
5. Save the category

The category will now only appear in the storefront top navigation for customers belonging to the selected segments. Customers outside those segments — including guests — will not see it.

### Module settings

Navigate to **Admin → Stores → Configuration → Customers → Customer Segment Menu**

| Setting | Description |
|---|---|
| **Enable Segment Menu** | Enables or disables the module's menu filtering functionality |
| **Enable Debug Logging** | Logs segment resolution details to `var/log/system.log` for troubleshooting |
| **Apply Customer Segments** | Manually triggers recalculation of all active customer segments |

---

## Manual Segment Recalculation

The **Apply Customer Segments** button in the admin configuration manually recalculates all active customer segments and writes results directly to `magento_customersegment_customer`, bypassing the message queue entirely.

> **Note:** Under normal operation, Magento handles segment recalculation automatically via the built-in cron job `customer_segment_match_customer_segment`. Use this button only in urgent situations or during testing and debugging when waiting for the next cron run is not practical.

---

## How It Works

```
Customer visits storefront
        │
        ▼
Module resolves current customer's segments
(via Magento HTTP Context)
        │
        ▼
Top menu rendering filters category nodes
against allowed segments per category
        │
        ├─ Customer belongs to allowed segment → category shown
        └─ Customer not in segment / guest    → category hidden
        │
        ▼
Cache variation applied per segment
(FPC / Redis compatible)
```

### Technical components

| Component | Purpose |
|---|---|
| `Plugin/TopmenuCustomerSegmentPlugin.php` | Filters top menu nodes during rendering |
| `Plugin/TopmenuSegmentCachePlugin.php` | Adds segment-based cache variation |
| `Plugin/CustomerSegmentContextPlugin.php` | Resolves and sets segment context in HTTP Context |
| `Model/Cache/Context/CustomerSegment.php` | Cache context identifier for segment variation |
| `Model/Config/Source/CustomerSegments.php` | Source model for segment multiselect on category |
| `Model/SegmentResolver.php` | Resolves active segments for the current customer |
| `Helper/Data.php` | Config helper for enable/debug flag reads |
| `Controller/Adminhtml/Segment/ApplyAll.php` | Admin AJAX controller for manual recalculation |
| `Block/Adminhtml/System/Config/ApplySegments.php` | Admin config button renderer |

---

## Compatibility

| Environment | Status |
|---|---|
| Adobe Commerce 2.4.x | ✔ Tested |
| Adobe Commerce B2B | ✔ Tested |
| PHP 8.1+ | ✔ Tested |
| Redis Full Page Cache | ✔ Tested |
| Varnish FPC | ✔ Compatible |
| Magento Open Source | ✗ Not supported (requires `Magento_CustomerSegment`) |

---

## Keywords

`magento2` `adobe-commerce` `customer-segment` `segment-menu` `category-visibility`
`b2b-navigation` `menu-restriction` `personalization` `redis-cache` `fpc`
`svextensions` `segment-based-navigation` `top-menu` `magento2-extension`

---

## Support

For issues or assistance, please contact **SVExtensions**.

---

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for release history.

