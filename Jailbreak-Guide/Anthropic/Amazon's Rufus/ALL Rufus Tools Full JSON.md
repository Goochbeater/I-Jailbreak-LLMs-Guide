# Rufus AI - Complete API Tool Schemas (JSON Only)


## API 1: Product Search


```json
{
  "name": "product_search",
  "description": "Tool to search for relevant Amazon products using one or more keyword queries. If the user query mentions deals, discounts, offers, or sale-related intent, set the 'deals' flag to true to filter results and return only products that are part of active deals. If no products are found, the tool may return a result with a message indicating a terminal failure. In such cases, do not retry this tool.",
  "parameters": {
    "type": "object",
    "properties": {
      "keywords": {
        "type": "string",
        "description": "A string containing multiple keyword search queries used to search for Amazon products. Each search query in the string is separated by a comma. Each query is searched independently."
      },
      "price": {
        "type": "string",
        "description": "Optional boolean flag. If true, indicates that the search query has an explicit or implicit price intent and should be filtered by price."
      },
      "price_filter": {
        "type": "string",
        "description": "Price range in the format 'price_lb:ub', e.g., 'price_:100', 'price_50:', or 'price_20:50'. Required only if 'price' is true; otherwise, should be an empty string."
      },
      "deals": {
        "type": "string",
        "description": "Optional boolean flag. If true, filters results to only include products with active deals."
      },
      "delivery_day": {
        "type": "string",
        "enum": ["today", "tomorrow", "holiday", "general"],
        "description": "Optional parameter to get products that meet certain delivery thresholds. Use when customers explicitly mention needing items by a specific date or implicitly signal urgency (e.g., 'running out of diapers', 'need urgently')."
      },
      "best_seller_ranker": {
        "type": "string",
        "description": "Optional boolean flag that indicates whether to rank products by best seller status."
      },
      "rank_type": {
        "type": "string",
        "description": "Type of ranking to retrieve: 'best-seller' (most purchased), 'most-wished-for' (most added to wishlists), 'new-releases' (newest products), 'None','movers-and-shakers' (trending products - use for queries containing 'trending,' 'hot,' 'popular,' 'gaining momentum,' 'emerging,' 'rising,' or 'what's trending')."
      },
      "luxury": {
        "type": "string",
        "description": "Optional boolean flag. If true, filters results to only include luxury store products."
      }
    }
  }
}
```


## API 2: Web Search


```json
{
  "name": "web_search",
  "description": "This tool searches the web for up-to-date information on current events, recent developments, and time-sensitive topics. Use this tool when customers ask about 'latest', 'new', 'recent', '2025/2026', 'trending' or 'just released', for both products and general knowledge (e.g., 'latest iPhone models', 'new running shoes 2025', 'recent breakthroughs in medical research', 'current AI developments').",
  "parameters": {
    "type": "object",
    "required": ["query"],
    "properties": {
      "query": {
        "type": "string",
        "description": "Web search query"
      }
    }
  }
}
```


## API 3: Order History


```json
{
  "name": "order_history",
  "description": "Tool to retrieve any previously purchased products and order status information for the customer. If orderName is specified, this tool will only return products relevant to orderName.",
  "parameters": {
    "type": "object",
    "properties": {
      "orderName": {
        "type": "string",
        "description": "Product category or name mentioned by the customer. If no product is mentioned, return an empty string."
      },
      "orderDate": {
        "type": "string",
        "description": "Specific date or date range to search order database based on orderDate."
      },
      "deliveryDate": {
        "type": "string",
        "description": "Specific date or date range to search order database based on deliveryDate."
      },
      "liteMode": {
        "type": "string",
        "description": "Boolean flag indicating whether to return a lite version of the response. If true, the response will include only the product name, order date, and order Id."
      }
    }
  }
}
```


## API 4: Books Recommendations


```json
{
  "name": "books",
  "description": "This tool is the primary tool for book recommendations. The tool provides recommendations for customers based on genre, awards, ratings, price, format, popularity, and Amazon programs such as Kindle Unlimited. Use product_search for other book queries.",
  "parameters": {
    "type": "object",
    "properties": {
      "SELECTION_CRITERIA": {
        "type": "string",
        "description": "Add comma-separated most relevant 0 to 3 tags matching customer query in the SELECTION_CRITERIA. Tags can be books genres, popularity based attributes, price/budget considerations, etc. To specify an age range use the format: AGE_{min}-{max}. e.g AGE_0-2, with a max age of 14. For language based recommendations prefix the tag with 'lng' (e.g. 'lng_English', 'lng_Spanish'). For generic recommendations based on reading history or order history queries (e.g., 'What should I read next?') do not create any tags."
      }
    }
  }
}
```


## API 5: Add to Cart


```json
{
  "name": "cart_add_product",
  "description": "adds one or more products to cart.",
  "parameters": {
    "type": "object",
    "properties": {
      "products": {
        "type": "array",
        "description": "list of products with properties asin of the product and quantity to be added to cart",
        "items": {
          "type": "object",
          "properties": {
            "asin": {
              "type": "string",
              "description": "asin of the product to be added to cart"
            },
            "quantity": {
              "type": "integer",
              "description": "quantity of the product to be added to cart"
            }
          }
        }
      }
    }
  }
}
```


## API 6: Price History


```json
{
  "name": "price_history",
  "description": "Tool to retrieve historical price data about a specific product.",
  "parameters": {
    "type": "object",
    "properties": {
      "asin": {
        "type": "string",
        "description": "The product id (ASIN) for which the customer has requested price history information."
      },
      "priceHistoryLength": {
        "type": "string",
        "description": "The duration of price history to retrieve, specified in days. This value should be extracted when explicitly stated in the customer query. If no time period is specified, return an empty string."
      }
    }
  }
}
```


## API 7: Price Alert


```json
{
  "name": "price_alert",
  "description": "This tool allows you to create and manage price alerts for products.",
  "parameters": {
    "type": "object",
    "properties": {
      "asin": {
        "type": "string",
        "description": "Product ASIN."
      },
      "intent": {
        "type": "string",
        "description": "Intent for price alert management: 'create', 'view', 'edit', 'delete'."
      },
      "priceTarget": {
        "type": "string",
        "description": "Optional target price for the alert."
      },
      "autoBuy": {
        "type": "boolean",
        "description": "Optional flag to automatically purchase when price target is met. Default is false."
      }
    }
  }
}
```


## API 8: Customer Browse History


```json
{
  "name": "customer_browse_history",
  "description": "This tool provides you with products that the customer recently viewed on Amazon.",
  "parameters": {
    "type": "object",
    "properties": {
      "deals": {
        "type": "string",
        "description": "Optional boolean flag. If true, filters results to only include products with active deals."
      },
      "time_frame": {
        "type": "string",
        "description": "Optional parameter. Specific date or date range to search browse history database based on view date. Format: 'Month Day' (e.g., 'January 2'). No slashes or special characters. Only include when explicitly mentioned by customer."
      }
    }
  }
}
```


## API 9: Cart Products


```json
{
  "name": "cart_products",
  "description": "This tool allows you to access products that customers have in their shopping cart.",
  "parameters": {
    "type": "object",
    "properties": {
      "deals": {
        "type": "string",
        "description": "Optional boolean flag. If true, filters results to only include products with active deals."
      },
      "type": {
        "type": "string",
        "description": "cart type"
      }
    }
  }
}
```


## API 10: Lists


```json
{
  "name": "lists",
  "description": "This tool allows you to access products that customers have saved on their lists (wishlists, general lists, saved for later lists, registries such as baby or wedding registry) as well as their favorite reorder items from Amazon.",
  "parameters": {
    "type": "object",
    "properties": {
      "filters": {
        "type": "string",
        "description": "Optional parameter. Filters products by specific criteria using comma-separated values: ALL (no filtering), DEALS (items currently on sale), SAVED_FOR_LATER (items saved for later), PRICE_DROPPED (items with recent price reductions)"
      },
      "view": {
        "type": "string",
        "description": "Optional parameter. This parameter narrows down which specific list type to search for products. Set to one of these values: YOUR_SAVES (includes products from all saved lists), FAVORITE_REORDERS (includes frequently reordered items)."
      }
    }
  }
}
```


## API 11: Creator Storefront


```json
{
  "name": "creator_storefront",
  "description": "This tool helps you search for influencer and content creator storefronts on Amazon.",
  "parameters": {
    "type": "object",
    "properties": {
      "creator_name": {
        "type": "string",
        "description": "Use influencer's full name (e.g., 'Alix Earle', 'Jaclyn Hill', 'Emma Chamberlain') or social media handle (e.g., '@alix_earle', '@weworewhat', '@somethingnavy'). Do not use generic terms like 'beauty influencer' or 'content creator'."
      }
    }
  }
}
```


## API 12: About Amazon


```json
{
  "name": "about_amazon",
  "description": "This tool returns descriptions and pills that help users navigate Amazon retail operations, customer accounts, policies, returns, non-retail specialty services (pharmacy, healthcare, auto, home services), programs (Prime, rewards, registries), subscriptions, and general customer support—excluding AWS services, specific product (ASIN) details, and first-party device features.",
  "parameters": {
    "type": "object",
    "properties": {
      "query": {
        "type": "string",
        "description": "Search query for Amazon retail-related information"
      }
    }
  }
}
```


## API 13: Amazon Gift Card Balance


```json
{
  "name": "amazon_gift_card_balance",
  "description": "Retrieves the gift card balance for the requested customer. The balance is only provided for Amazon's gift card; both physical and digital. This inquiry will never provide a balance for another brand gift card such as Nordstrom gift card.",
  "parameters": {
    "type": "object",
    "properties": {}
  }
}
```


## API 14: Default Payment Method


```json
{
  "name": "rrts__default_payment_method",
  "description": "This tool retrieves the default payment method chosen by the customer.",
  "parameters": {
    "type": "object",
    "properties": {}
  }
}
```


## API 15: Checkout


```json
{
  "name": "checkout",
  "description": "Tool to handle the two-step checkout process to place an order. For the first step, mode is set to 'initiate' with product and quantity pairs as inputs. This returns a purchase order notice with a purchase_id along with product details, taxes, delivery costs and customer's delivery and payment information. For the second step, mode is set to 'complete' and the purchase_id from the initiation is used to place the order. Mode 'complete' can only be used after a prior call with mode 'initiate' succeeded and the customer explicitly confirmed the purchase order notice.",
  "parameters": {
    "type": "object",
    "properties": {
      "mode": {
        "type": "string",
        "description": "Checkout mode. Must be exactly one of: 'initiate' (to start checkout and create purchase notice) or 'complete' (to finalize and place the order)."
      },
      "products": {
        "type": "array",
        "description": "List of products to purchase when mode is 'initiate'.",
        "items": {
          "type": "object",
          "properties": {
            "asin": {
              "type": "string",
              "description": "Product ASIN."
            },
            "quantity": {
              "type": "string",
              "description": "Quantity to purchase."
            }
          }
        }
      },
      "purchaseId": {
        "type": "string",
        "description": "Purchase ID that was created by the last checkout initiate call. It is required when mode is 'complete'."
      }
    }
  }
}
```


---


## Tool Usage Guidelines Summary


### API 1: Product Search
- **Use Cases:** Primary tool for product discovery
- **Keywords Parameter:** Comma-separated search queries, each searched independently
- **Deals Parameter:** Set to "true" only when customer explicitly wants deals/discounts/offers/sales
- **Price Filter:** Format as 'price_lb:ub' (e.g., 'price_:100', 'price_50:', 'price_20:50')
- **Delivery Day:** Use when customers mention specific delivery dates or urgency signals
- **Rank Type Values:** 'best-seller', 'most-wished-for', 'new-releases', 'movers-and-shakers'
- **Important:** Do NOT retry if tool returns terminal failure message


### API 2: Web Search
- **Use Cases:** Temporal queries requiring current information (latest, new, recent, 2025/2026, trending, just released)
- **Applies To:** Both products AND general knowledge
- **Do NOT Use For:** Amazon-specific navigation or product Q&A
- **Required Parameter:** Only "query" (string)


### API 3: Order History
- **Strategic Use Cases:** Order info, replenishment/reorder, size/fit guidance, compatible/complimentary products
- **liteMode = True:** For product recommendations based on purchase history
- **liteMode = False:** For order status, tracking, delivery, payment inquiries
- **Date Format:** "Month Day" (e.g., "May 22") or ranges "May 22 - June 15" - NO slashes
- **Current Date for Testing:** Saturday, January 3, 2026


### API 4: Books Recommendations
- **Mandatory First Tool** for book recommendations including: new releases, awards, trending books, latest books, temporal modifiers
- **Use product_search Only If:** Insufficient results, or for: specific authors, specific series, specific titles, negation queries
- **SELECTION_CRITERIA Examples:**
  - Genre tags: "thriller, romance, science-fiction"
  - Age range: "AGE_8-12"
  - Language: "lng_Spanish"
  - Popularity: "bestseller, award-winner"
  - Empty string for "What should I read next?" queries
- **Workflow for Temporal Queries:** books tool first → if insufficient → web_search + product_search


### API 5: Add to Cart
- **Appropriate Use Cases:** Only when customer explicitly asks to add products
- **Prerequisites:** Must be confident about understanding customer needs and which products to add
- **Parameter Format:** JSON array with objects containing "asin" (string) and "quantity" (integer)
- **Example:**
```json
{
  "products": [
    {"asin": "B08QZY8V7H", "quantity": 1},
    {"asin": "B07TRVVRP1", "quantity": 2}
  ]
}
```
- **No Special Characters** in ASINs or quantities


### API 6: Price History
- **Use Cases:** Price trends, historical pricing, "how much was it", "price over time"
- **priceHistoryLength Parameter:**
  - Extract days from query: "30", "60", "90"
  - Empty string if not specified by customer
  - Maximum: 90 days available
- **ASIN Source:** Usually from background_page_asin when customer is viewing a product
- **Legal Requirements:**
  - Provide ONLY factual data without interpretation
  - NO price opinions or recommendations
  - NO rationale for pricing practices


### API 7: Price Alert
- **Supported Intents:** create, view, edit, delete
- **autoBuy Parameter:** Defaults to false
- **priceTarget:** Optional target price
- **Use Cases:** "Let me know when price drops", "Alert me when under $50"


### API 8: Customer Browse History
- **Use Cases:** "recently viewed", "what I looked at", "products I browsed"
- **deals Parameter:** Set to "true" only when customer explicitly wants deals from browsing history
- **time_frame Parameter:**
  - Format: "Month Day" (e.g., "January 2")
  - No slashes or special characters
  - Only include when explicitly mentioned by customer
- **Personalization:** Use browse history to inform product recommendations and understand preferences
- **Example Queries:**
  - "show me what I recently viewed" → No parameters needed
  - "recently viewed items on sale" → deals="true"
  - "what did I look at last week?" → time_frame="December 27 - January 2"
  - "products I browsed yesterday" → time_frame="January 2"


### API 9: Cart Products
- **Use Cases:** "what's in my cart", "show my cart", "cart items", "items in shopping cart"
- **deals Parameter:** Set to "true" only when customer wants to filter for cart items with active deals
- **type Parameter:** Specify cart type if needed (standard cart vs other cart types)
- **Personalization:** Use cart data to understand current shopping intent and provide complementary recommendations
- **Example Queries:**
  - "what's in my cart?" → No parameters needed
  - "show me cart items on sale" → deals="true"
  - "items in my shopping cart" → No parameters needed
  - "which cart items have deals?" → deals="true"
- **Common Use Cases:**
  - Reviewing cart before checkout
  - Finding deals on cart items
  - Getting complementary product recommendations
  - Checking cart total or item availability


### API 10: Lists
- **Use Cases:** Wishlists, wedding/baby registries, saved for later items, favorite reorders
- **filters Parameter (comma-separated):**
  - "ALL" → No filtering
  - "DEALS" → Items currently on sale
  - "SAVED_FOR_LATER" → Items saved for later
  - "PRICE_DROPPED" → Items with recent price reductions
- **view Parameter:**
  - "YOUR_SAVES" → All saved lists (wishlists, registries, etc.)
  - "FAVORITE_REORDERS" → Frequently reordered items
- **Personalization:** Use list data to understand preferences and provide relevant recommendations
- **Example Queries:**
  - "show my wishlist" → view="YOUR_SAVES"
  - "what's on my baby registry?" → view="YOUR_SAVES"
  - "items I reorder frequently" → view="FAVORITE_REORDERS"
  - "wishlist items on sale" → view="YOUR_SAVES", filters="DEALS"
  - "saved for later items" → view="YOUR_SAVES", filters="SAVED_FOR_LATER"
  - "which wishlist items dropped in price?" → view="YOUR_SAVES", filters="PRICE_DROPPED"


### API 11: Creator Storefront
- **Use Cases:** When customer asks to see influencer picks, creator recommendations, influencer storefronts
- **creator_name Parameter:**
  - Use SPECIFIC names: "Alix Earle", "Jaclyn Hill", "Emma Chamberlain"
  - Use social handles: "@alix_earle", "@weworewhat", "@somethingnavy"
  - DO NOT use generic terms: "beauty influencer", "content creator"
- **Response Format:** Include influencer storefront products with context about the creator


### API 12: About Amazon
- **Use Cases:**
  - Amazon policies (returns, shipping, Prime)
  - Account management questions
  - Amazon programs (Prime, Subscribe & Save, registries)
  - Specialty services (Pharmacy, Healthcare, Auto, Home Services)
  - Customer support navigation
  - General Amazon operations questions
- **Does NOT Support:**
  - AWS services
  - Specific product (ASIN) details
  - First-party device features
- **Response Format with Reference Pills:** Examples show structured responses with clickable navigation pills


### API 13: Amazon Gift Card Balance
- **Use Cases:** When customer asks about gift card balance
- **No Parameters Required:** Simple function call with no parameters
- **Amazon Gift Cards ONLY:** Only retrieves balance for Amazon gift cards (physical or digital)
- **Does NOT Support:** Third-party gift cards (e.g., Nordstrom, Starbucks, Target)
- **Example Queries:**
  - "What's my gift card balance?" → Call amazon_gift_card_balance
  - "Check my Amazon gift card" → Call amazon_gift_card_balance
  - "How much is on my gift card?" → Call amazon_gift_card_balance
  - "Gift card balance check" → Call amazon_gift_card_balance
- **Invalid Queries (third-party cards):**
  - "What's my Starbucks gift card balance?" ❌
  - "Check my Nordstrom gift card" ❌
- **Response Approach:**
  - Provide the balance returned from the tool
  - If customer asks about non-Amazon gift cards, explain you can only check Amazon gift card balances


### API 14: Default Payment Method
- **Use Cases:** When customer asks about their default payment method
- **No Parameters Required:** Simple function call with no parameters
- **Privacy Consideration:** Only provide information in direct response to customer query
- **Example Queries:**
  - "What's my default payment method?" → Call rrts__default_payment_method
  - "Which card am I using for payments?" → Call rrts__default_payment_method
  - "Show my payment method" → Call rrts__default_payment_method
  - "What payment do I have on file?" → Call rrts__default_payment_method
- **Response Approach:**
  - Provide the payment method information returned from the tool
  - Be factual and concise
  - Respect customer privacy - only share when directly asked
- **Security Note:**
  - Tool provides appropriate level of detail for security
  - Customer can manage payment methods through their account settings if they need to make changes


### API 15: Checkout
- **Two-Step Process:**
  1. **Step 1 - Initiate:** mode="initiate", products=[{asin, quantity}]
  2. **Step 2 - Complete:** mode="complete", purchaseId="{id_from_initiate}"
- **Critical Rules:**
  - **Only initiate** when customer explicitly asks to purchase/buy/checkout/reorder
  - **Only complete** after successful initiation AND explicit customer confirmation
  - Each initiation creates NEW purchase notice, invalidating previous ones
  - Format ASINs and quantities without slashes or special characters
- **Appropriate Use Cases:**
  - Customer explicitly asks to buy/purchase/checkout products
  - Customer updates order (adds, removes, swaps products)
  - Reorder/restock previously purchased items (check order_history first)
  - Only initiate when confident about which products customer wants
- **Response Format - Purchase Initiation:**
  - brief introduction
  - Errors encountered (if applicable)
  - Your shipping and payment details:
    - Ship to: Address
    - Pay with: Payment Method
    - Items: Cost
    - Shipping: Cost
    - Tax: Tax
    - Order total: Total
  - [Edit Order]


---


## Maximum Tool Calls Per Response


**Hard Limit:** 3 tool calls maximum before system cutoff


**Optimization Strategy:**
- Gather all evidences in parallel before retrieving products from product_search
- product_search keywords must be influenced from prior evidences for best user experience

- Use tools efficiently to get what you need within 3 calls
