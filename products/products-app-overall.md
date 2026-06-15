# Products App Overall

## Source Notes

- Frontend app: `hotwax/products`
- Supporting product APIs: OMS product endpoints and the PIM product-data component
- Requirements context: retailer product-data requirements for bundles, SKU aliases, fulfillment metadata, trade metadata, financial attributes, product-store scoping, and product data quality

## Press Release

### HotWax launches Products, a dedicated app for operational product data management

HotWax Commerce today announced Products, a new OMS app that gives retailers a focused workspace for managing the product data that keeps orders, inventory, fulfillment, storefront sync, and finance workflows moving.

Most product systems are built for merchandising. They help teams describe products, publish product pages, and manage storefront content. But the OMS depends on a different layer of product truth: SKU identifiers, variant relationships, kit components, substitute products, ship-ready dimensions and weights, handling flags, tax and financial mappings, product-store visibility, marketplace mappings, and data-quality issues that can stop fulfillment downstream.

Products gives operations, integration, and merchandising teams one place to find, review, and correct that operational product data. The app combines a searchable product workbench, product creation, detailed product editing, variant and family management, identifier management, kit and substitute management, tags, categories, prices, Shopify shop-product mappings, product-store mapping management, import history, product history, missing-field checks, duplicate identifier resolution, and product search-index diagnostics.

Three capabilities make this more than a product edit screen. Users can enter package dimensions and visually inspect the box shape in 3D before bad shipping data reaches carriers or 3PLs. Good-identification records give every product a flexible identity layer for mapping SKUs, UPCs, barcodes, HS codes, marketplace IDs, ERP IDs, and partner-specific aliases to any external system. Product-store mapping management gives retailers control over which products are sellable in which stores, brands, channels, and Shopify shops, making cross-brand selling operationally manageable instead of a hidden integration rule.

With Products, retailers can move product-data cleanup out of spreadsheets and disconnected integration tickets and into a service-backed OMS workflow.

### The Problem

Retail product data now lives across too many systems. Shopify or a marketplace may own the customer-facing catalog. An ERP may own cost, tax, and ledger fields. A 3PL may need boxed dimensions, weights, hazmat flags, heat sensitivity, and customs metadata. The OMS needs SKU relationships, fulfillment components, substitute products, store mappings, and product-store visibility.

When those systems disagree, order operations pay the price.

A single kit SKU can arrive from a storefront while the warehouse needs component SKUs. Duplicate UPCs or SKUs can break integrations. A missing boxed weight can cause wrong postage. Incorrect dimensions can make a carton look plausible in a form but fail once a carrier or warehouse sees the physical package. A missing HS code can block international fulfillment. An acquired brand may use a SKU format that does not match the retailer's internal SKU. A merchandising team may update a product in Shopify, but the OMS still lacks the operational metadata needed to broker, reserve, transmit, or reconcile the order.

The common workaround is a patchwork of spreadsheets, one-off SQL fixes, admin screens, and integration-specific notes. That does not scale when retailers add brands, channels, marketplaces, or fulfillment partners.

### The Solution

Products creates a practical product data management layer inside HotWax OMS.

The app starts with a product workbench for search and review. Users can search by product, parent product, SKU, internal name, or identifier; filter by product type, product store, product kind, tag, and product group; sort by created, updated, or alphabetical order; and take bulk actions such as tagging selected products.

From there, users can open a product detail page that organizes operational data into editable sections:

- Display fields, descriptions, product type, image, and brand fields
- Variant and product-family navigation
- Feature axes and feature values for virtual and variant products
- Identifiers such as SKU, UPC, HS code, and other good-identification types
- Kit components and product associations
- Substitute products and policy controls
- Tags and categories
- Prices and currency
- Shopify shop-product and inventory-item mappings
- Product-store mappings for brand, channel, and cross-store selling control
- Shipping and handling metadata, including weights, dimensions, box type, and UOMs
- 3D package visualization for entered box dimensions
- Product history and import history for investigation

The app also includes data-quality workflows. Duplicate identifier screens help teams find duplicate SKU or UPC groups and resolve them without hand-editing each product separately. Missing-data screens show coverage for required product fields such as UPC, SKU, image, brand, primary category, and tags, then let users drill into the products that need cleanup.

Settings provide product-store context, OMS instance context, search-index health, product counts, and a rebuild action when the index looks stale.

### Why This Matters

Products makes HotWax OMS a better operational hub because it turns product data into an actionable workflow.

Retailers no longer need to treat product data errors as mysterious integration failures. If a product cannot be fulfilled cleanly, the team can inspect the product, see its identifiers and relationships, check its mappings, update its handling metadata, review the package dimensions visually, review history, and run data-quality screens from the same app.

This is especially important for retailers that operate multiple brands, sell bundles and kits, fulfill through multiple warehouses or 3PLs, and keep Shopify or marketplaces as the customer-facing catalog. The OMS can own the fulfillment-ready product data without forcing the storefront, ERP, or warehouse system to become the master for every operational attribute. Good identifications let each system keep its own identifier while HotWax maintains the translation layer, and product-store mappings let a retailer decide which brands and products participate in each selling channel.

### Customer Experience

An operations user starts in the Products workbench and filters to a product store. They search for a SKU that failed in an integration, open the product family, and confirm the variant, UPC, Shopify inventory item mapping, shipping weight, box dimensions, and substitute products. They use the 3D package preview to see whether the entered dimensions resemble the actual shipping box. If the SKU is sold across multiple brands or Shopify shops, they review product-store and shop mappings to confirm where it is sellable and which external identifiers are used. If the SKU is part of a kit, they review its component relationships. If the UPC is duplicated, they use the duplicate-resolution workflow. If the product is missing key data, they use the missing-data drilldown to clean up the impacted products before orders reach the warehouse.

Instead of bouncing between Shopify, spreadsheets, integration logs, and database support requests, the user works from a purpose-built OMS product workspace.

### Launch Scope

The current Products app establishes the core product data management experience:

- Product workbench with search, filters, facets, infinite scrolling, and bulk tagging
- Product creation flow with display, tags, dates, shipping, inventory policy, identifiers, categories, prices, and features
- Product detail workspace for core fields, variants, features, identifiers, categories, prices, Shopify mappings, product-store mappings, shipping, handling, tags, components, substitutes, and history
- 3D package-dimension visualization for shipping box review
- Data quality workflows for missing fields and duplicate identifiers
- Import history for recent product update syncs
- Settings for product-store selection, product index status, and index rebuilds
- Service-backed writes for product fields, identifiers, associations, features, prices, categories, tags, and Shopify mappings

The next expansion areas are deeper kit/BOM validation, nested kit handling, kit ATP visibility, richer good-identification governance, trade metadata editing, financial mapping fields, product import/export templates, product data review documents, and stronger brand/legal-entity scoping.

## FAQ

### What is Products?

Products is a HotWax OMS app for operational product data management. It is designed for the product data that order orchestration depends on: identifiers, variants, kit components, substitutes, tags, categories, prices, Shopify mappings, product-store mappings, shipping metadata, package dimensions, data quality, and product history.

### Is this meant to replace Shopify, a merchandising PIM, or an ERP?

No. Products is not trying to replace the storefront catalog, merchandising PIM, or ERP financial master. It gives the OMS a focused place to manage the operational product data needed for fulfillment, routing, inventory reservation, integration mapping, warehouse transmission, and reconciliation.

The right ownership model may still vary by retailer. Shopify can remain the customer-facing catalog. The ERP can remain the financial system of record. Products gives HotWax the service-backed product data layer required to make OMS workflows reliable.

### Why does an OMS need its own product data app?

The OMS needs fields and relationships that are often not cleanly owned by a storefront or ERP. Examples include kit components, substitute SKUs, Shopify inventory-item mappings, boxed weights, shipping dimensions, handling flags, product-store visibility, operational tags, and duplicate identifier cleanup.

When that data is wrong, orders fail operationally. Products gives teams a place to detect and fix those issues before they become warehouse or customer-service problems.

### What can users do in the product workbench?

Users can search and filter the product catalog, review product rows, apply product-store and type filters, inspect tag and product-group facets, sort by common operational views, select products in bulk, and apply tags to selected products.

The workbench is built on the product search index, so it is intended for fast product discovery rather than one-product-at-a-time navigation.

### What can users manage on a product detail page?

The detail page brings together the product's operational record. Users can manage display fields, product type, brand, descriptions, image, variant navigation, feature applications, identifiers, categories, tags, prices, Shopify shop-product mappings, product-store mappings, substitute products, kit components, shipping and handling fields, and history.

For virtual and variant products, the page also helps users move between the parent product and variants instead of treating every SKU as an isolated record.

### Why does 3D package visualization matter?

Package dimensions are easy to mistype and hard to evaluate as raw numbers. A user may enter length, width, and height that technically save but obviously do not match the product's expected carton shape.

The 3D box visualization turns dimension entry into visual validation. Operations users can immediately see whether the package looks like a shoebox, bottle case, poster tube, cosmetics carton, or oversized parcel before the data is used by carriers, 3PLs, rate shopping, or warehouse transmission.

### How does Products handle variants and product families?

Products groups a product family around the parent or virtual product and lets users switch between family members from the detail page. It includes feature-axis support so users can work with attributes like color and size, apply features, and add missing variants when a family needs another valid combination.

### How does Products handle kits, bundles, and components?

Products uses product associations to model component relationships and substitutions. The app already exposes component and substitute management in the product detail workflow.

The strategic direction is to make this the operational home for OMS-owned BOM management: storefronts and marketplaces can pass a single kit SKU, while the OMS stores the component relationships needed for warehouse transmission, inventory planning, and ATP logic.

### Does Products support kits-within-kits?

Nested kit support should be treated as an expansion area, not as completed launch scope. The data model can represent product relationships, but the productized workflow still needs validation for nested structures, loop prevention, effective dating, clear error messaging, and downstream explosion behavior.

### How does this connect to available-to-promise for kits?

Products should own or expose the BOM data that inventory logic needs. Once component relationships are reliable, ATP can calculate kit availability from the most constrained component SKU.

The Products app does not need to become the inventory calculation engine. Its role is to make the component data reviewable, editable, auditable, and safe enough for ATP and fulfillment services to trust.

### How does Products help with gifts with purchase and substitutions?

Products already exposes substitute relationships. That creates a foundation for more advanced fallback hierarchies, such as replacing an out-of-stock gift SKU with the next eligible substitute based on priority, channel, brand, campaign, or inventory threshold.

The next step is to make substitution intent more explicit so operations teams can distinguish a general substitute from a promotion-specific fallback rule.

### How does Products support fulfillment metadata?

Products gives users a place to manage shipping and handling fields such as boxed weight, product weight, dimensions, shipment box type, and units of measure. These fields are critical because storefront product weight is often not the same as the ship-ready package weight a 3PL or carrier needs.

The roadmap should extend this into clearer handling metadata for hazmat, heat sensitivity, country of origin, HS codes, and other fulfillment or customs attributes that are not always native to the storefront catalog.

### How does Products support financial and legal-entity metadata?

Products already has foundations such as brand, product prices, taxable flags, categories, and identifiers. Retailers also need product-level tax codes, GL account mappings, legal entity codes, and brand identity preserved through order lines and downstream exports.

The open design question is whether those fields should live as strong product entities, product attributes, or integration-specific mappings. The PR FAQ should position this as an intentional expansion area because the data has financial and reporting consequences.

### How do good identifications make Products easier to integrate?

Good identifications are the product identity layer for integrations. A product can have many identifiers at the same time: internal SKU, UPC, barcode, HS code, Shopify variant id, marketplace SKU, ERP item id, warehouse item id, vendor style number, or brand-specific alias.

That means HotWax does not need every external system to use the same SKU. Products can store the identifiers each system expects, and integration logic can map reliably through configured good-identification types instead of hard-coded translation tables.

### How does Products support SKU aliases?

The product APIs can use good-identification records for multiple identifier types. That gives the system a natural place to store SKU, barcode, HS code, and alias values.

For acquired brands or marketplace-specific SKU formats, Products should productize alias management so users can map external SKUs to internal SKUs without requiring ERP database changes or hidden integration logic.

### How does product-store mapping support cross-brand selling?

Product-store mapping gives retailers a controlled way to decide where a product is active, sellable, and operationally valid. This matters when one company runs multiple brands, Shopify shops, marketplaces, or regional storefronts from the same OMS.

Instead of copying products into separate systems or hiding cross-brand selling rules in integration code, Products can expose mappings by product store, brand, channel, and shop. A shared product can be sold through multiple storefronts while still preserving store-specific identifiers, inventory behavior, pricing context, and downstream fulfillment rules.

### How does Products work with Shopify?

Products includes Shopify shop-product mapping management, including shop and inventory-item mappings. This matters because Shopify may remain the source for customer-facing product presentation, while HotWax needs a reliable operational mapping between Shopify products, OMS products, variants, and inventory items.

The app should also become the place to inspect and correct product sync outcomes when Shopify product updates are imported into the OMS.

### How does Products help data quality?

Products includes dedicated data-fix screens for duplicate identifiers and missing required fields. Duplicate workflows can identify groups such as duplicate SKUs or UPCs and resolve them in a batch. Missing-data workflows show coverage for key fields and let users drill into products that need cleanup.

This is important because product errors often become visible only after an order fails. Products moves that cleanup earlier in the process.

### What does import history do?

The Imports page shows recent product update history so users can investigate syncs by product, SKU, barcode, or shop. It gives teams a starting point for answering whether product data entered the OMS, when it arrived, and whether the issue is in the source data, mapping, or downstream processing.

### What does product history do?

Product history gives users an audit trail for product changes that are recorded by the backend. This is especially useful for identifier cleanup, where users need to understand when a SKU or UPC changed and why an integration started behaving differently.

### What is the search-index role?

Products relies on a product search index for fast discovery, filtering, facets, missing-field checks, and duplicate detection. Settings expose index health, product count, and a rebuild action so admins can recover when results look stale or empty.

### What requirements does this address?

Products addresses a broad set of operational product data needs:

- Manage fulfillment-ready product metadata outside the storefront
- Visualize package dimensions in 3D before shipping data reaches carrier or warehouse workflows
- Store and review kit/BOM relationships
- Support good-identification management for SKU identifiers, aliases, barcodes, HS codes, and external system IDs
- Track product mappings needed for Shopify, product stores, brands, marketplaces, and other channels
- Prepare for kit ATP by making component relationships visible and governed
- Store ship-ready dimensions, weights, and handling metadata
- Support trade metadata such as HS codes and country of origin
- Preserve brand, legal entity, and financial mapping context for downstream reconciliation
- Improve data quality with missing-field and duplicate-identifier workflows
- Support faster brand or channel onboarding by making product setup reviewable
