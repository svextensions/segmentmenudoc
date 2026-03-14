# Changelog

All notable changes to SVExtensions_SegmentMenu will be documented in this file.

## [1.0.0] - 2026-03-14

### Added
- Segment-based top navigation menu visibility
- Category attribute `allowed_customer_segments` for assigning allowed segments per category
- Guest customer restriction — segment-restricted categories hidden from unauthenticated users
- HTTP Context cache variation per customer segment for FPC and Redis compatibility
- `TopmenuCustomerSegmentPlugin` — filters top menu nodes based on segment membership
- `TopmenuSegmentCachePlugin` — adds segment identifier to cache context
- `CustomerSegmentContextPlugin` — resolves and sets active segments in HTTP Context
- `SegmentResolver` — resolves active customer segments for current session
- Admin configuration panel under Stores → Configuration → Customers → Customer Segment Menu
  - **Enable Segment Menu** toggle
  - **Enable Debug Logging** toggle
  - **Apply Customer Segments** button for manual segment recalculation
- Manual segment recalculation controller `Adminhtml/Segment/ApplyAll`
  - Bypasses message queue — writes directly to `magento_customersegment_customer`
  - Works without RabbitMQ on local environments
  - Protected by admin session and ACL resource `SVExtensions_SegmentMenu::apply_segments`
- Multi-store and multi-website support
- Compatible with Adobe Commerce B2B environments