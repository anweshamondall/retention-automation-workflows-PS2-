# Workflow: Abandoned Cart Retention Campaign

## Objective

Re-engage users who added products to their cart but did not complete the purchase. The goal is to recover lost conversions through a time-sequenced, multi-touch communication flow across WhatsApp and email.

---

## Trigger

**Event:** User adds a product to cart but does not complete checkout within a defined inactivity window.

**Trigger Condition:**
- `cart_created = true`
- `purchase_completed = false`
- Inactivity window: 1–2 hours after cart creation (configurable)

**Platform:** Customer.io / Interakt

---

## Target Segment

Users who:
- Have an active cart with at least one item
- Have not visited the checkout confirmation page
- Have a valid WhatsApp number or email address on record

---

## Workflow Sequence

```
[Trigger: Cart Abandoned]
          │
          ▼
   Wait: 1–2 hours
          │
          ▼
┌─────────────────────────────┐
│  Touch 1: WhatsApp Message  │
│  Channel: Interakt          │
│  Content: Soft reminder     │
│  "Hey {{name}}, you left    │
│  something behind!"         │
│  CTA: Return to Cart (link) │
└─────────────────────────────┘
          │
          ▼
  Did user complete purchase?
     │              │
    YES              NO
     │              │
     ▼              ▼
  EXIT          Wait: 24 hours
                    │
                    ▼
        ┌───────────────────────┐
        │  Touch 2: Email       │
        │  Platform: Customer.io│
        │  Content: Reminder +  │
        │  product image        │
        │  CTA: Shop Now (UTM)  │
        └───────────────────────┘
                    │
                    ▼
        Did user complete purchase?
             │              │
            YES              NO
             │              │
             ▼              ▼
           EXIT         Wait: 48 hours
                            │
                            ▼
                ┌───────────────────────┐
                │  Touch 3: WhatsApp    │
                │  Content: Urgency /   │
                │  limited stock nudge  │
                │  CTA: Complete Order  │
                └───────────────────────┘
                            │
                            ▼
                Did user complete purchase?
                     │              │
                    YES              NO
                     │              │
                     ▼              ▼
                   EXIT        EXIT (workflow ends)
```

---

## Message Templates

### Touch 1 — WhatsApp (Soft Reminder)

**Template Name:** `cart_reminder_t1`
**Header:** Product image (hosted via ImgBB)
**Body:**
```
Hey {{1}}, looks like you left something in your cart! 🛒

Your items are waiting — don't miss out.
```
**CTA Button:** `Complete Your Order` → `{{cart_url}}`

---

### Touch 2 — Email

**Subject Line:** `You forgot something, {{first_name}} 👀`
**Preview Text:** `Your cart is still waiting for you.`
**Content:** HTML template with product image block, headline, and CTA button
**CTA:** `Go Back to Cart` → UTM-tagged URL
`utm_source=email&utm_medium=retention&utm_campaign=abandoned_cart_t2`

---

### Touch 3 — WhatsApp (Urgency)

**Template Name:** `cart_reminder_t3`
**Body:**
```
Hi {{1}}, just a heads up — items in your cart may sell out soon.

Complete your order before it's gone! ⚡
```
**CTA Button:** `Order Now` → `{{cart_url}}`

---

## Configuration Notes

- All WhatsApp templates registered and approved on **Meta Business Suite** before workflow activation
- Dynamic variables (`{{1}}`, `{{cart_url}}`) mapped to customer data fields in Interakt
- Email HTML template uploaded to Customer.io with UTM links embedded
- Workflow tested with internal test numbers and email addresses before live deployment
- Delivery logs monitored post-launch to catch any template mapping errors

---

## Tracking

| Touch | Channel | UTM Campaign Tag | Platform |
|---|---|---|---|
| Touch 1 | WhatsApp | `abandoned_cart_t1` | Interakt |
| Touch 2 | Email | `abandoned_cart_t2` | Customer.io |
| Touch 3 | WhatsApp | `abandoned_cart_t3` | Interakt |

Performance metrics tracked via Meta Analytics Dashboard and Excel-based campaign reporting sheets.
