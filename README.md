# Shopify Custom Product Labels

This Shopify theme customization enables dynamic, scalable product **variant-level labels** using native metafields and Shopify best practices.

---

## 🔍 Overview

This solution allows clients to display variant-specific product badges such as:

- **New**
- **Exclusive**
- **Best Seller**
- **Sale** *(auto-assigned when a `compare_at_price` is present)*
- **Low Stock** *(auto-assigned when inventory < 10)*
- **Sold Out** *(auto-assigned)*

Each label can be styled per variant with customizable **text**, **text color**, and **background color**. Labels update dynamically when a different variant is selected on the product page.

---

## 🛍️ For Clients: Managing Product Variant Labels

To manage custom labels for your product variants:

### Step-by-Step Instructions

1. Go to **Admin > Products** in your Shopify store.
2. Select a product that has 1 or more variants.
3. Scroll down to the **Variants** section.
4. Check the box next to the variant(s) you wish to edit, then click **Bulk Edit**.
5. In the Bulk Editor, click the **"Columns"** button at the top right.
6. Scroll down or search for the following **Metafields**, then enable them:
   - `Label ID`
   - `Label Text Color`
   - `Label Background Color`
7. Now you'll see these fields in the table for each variant:
   - **Label ID**: Add the label name (e.g., `Exclusive`, `New`, etc.)
   - **Label Text Color**: Choose a color or input a hex code (e.g., `#ffffff`)
   - **Label Background Color**: Choose a color or input a hex code (e.g., `#000000`)
8. You can also click **"Show all metafields"** for an individual variant to manually set the same fields.

> ✅ These values can be edited anytime and will update automatically on the storefront.

---

## 💻 For Developers: Maintaining the Feature

### Modified Theme Files

- `assets/product-info.js` — dispatches `variant:change` event
- `assets/global.js` — listens for variant changes and updates label dynamically
- `sections/main-product.liquid` — exposes metafield data to frontend via JSON
- `snippets/price.liquid` — renders built-in and custom labels conditionally
- `locales/en.default.json` — adds new translation strings for Low Stock badge
- `config/settings_schema.json` — adds color scheme options for Low Stock badge in theme editor

### Label Rendering Logic

- **Custom Labels** are rendered using variant metafields:  
  - `custom_labels.label_id`  
  - `custom_labels.label_text_color`  
  - `custom_labels.label_bg_color`

- **Auto-assigned Labels**:
  - `Sold Out`: Shown if the variant is not available
  - `Low Stock`: Shown if `inventory_quantity < 10`
  - `Sale`: Shown if `compare_at_price > price`
  
### 🧭 Badge Priority (Override Rules)

When multiple conditions are true, labels are rendered in the following order of priority:

1. **Sold Out** – Always overrides all other labels.
2. **Low Stock** – Shown only if inventory is less than 10 and the product is available. Overrides Sale and Custom labels.
3. **Custom Label** – Defined via variant metafields. Overrides the Sale label, but is overridden by Sold Out or Low Stock.
4. **Sale** – Shown if `compare_at_price` exists and no other label is active.

- When a variant is changed, a custom event updates the label accordingly via JavaScript.

---

## Metafield Setup (Shopify Admin)
To use the custom product labels, the following variant metafields are defined in `Settings > Custom Data > Variants`:

1. **Label Text**
   - Namespace: `custom`
   - Key: `label_text`
   - Type: Single line text

2. **Label Background Color**
   - Namespace: `custom`
   - Key: `label_bg_color`
   - Type: Color

3. **Label Text Color**
   - Namespace: `custom`
   - Key: `label_text_color`
   - Type: Color

Assign these metafields to each variant under the variant editing screen.

---

## 📝 Notes

- Built on top of the **Shopify Dawn theme (v15.3.0)**.
- No build tools or theme compilation used.
- Fully Git-integrated for version tracking and developer collaboration.
- Structured as per best practice commit guidelines.

---