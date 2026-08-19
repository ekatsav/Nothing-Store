## Sequence Diagram for Product Search + Order
### Source: Unit 3 Assessment

This diagram similarly follows the user in finding a product and placing an order, but with more focus on the exact order of actions in terms of which part of the system talks to which.

#### Diagram

```mermaid
sequenceDiagram
    actor C as Customer
    participant F as Frontend
    participant B as Backend
    participant DB@{ "type": "database", "alias": "Database" }
    participant PG as Payment Gateway

    C->>F: Browse / Search Products
    F->>B: GET /products?query=...
    B->>DB: Query products + stock levels
    DB-->>B: Product list with stock

    alt Known User
        B->>DB: Query recommendation data (account/digital footprint)
        DB-->>B: Customer preferences
        B-->>F: Product list + personalised recommendations
    else Unknown User
        B-->>F: Product list + popular/trending items
    end

    F-->>C: Display products & recommendations

    C->>F: Select product
    F->>B: GET /products/{id}
    B->>DB: Query product details & stock
    DB-->>B: Product data

    alt In Stock
        B-->>F: Product details (available)
        F-->>C: Display product page (name, price, images, availability, information)
        C->>F: Add to cart
        F->>F: Update local cart state
    else Out of Stock
        B-->>F: Product details (unavailable)
        F-->>C: Display out-of-stock message
        C->>F: Continue browsing
        Note over C,F: Loops back to Browse / Search Products
    end

    C->>F: Proceed to checkout
    F-->>C: Display checkout form

    loop Until input valid
        C->>F: Submit delivery & payment details
        F->>F: Client-side validation
        alt Input Invalid
            F-->>C: Display validation errors
        end
    end

    F->>B: POST /orders (cart, address, payment token)
    B->>B: Server-side validation & sanitisation

    loop Until payment authorised
        B->>PG: Authorise payment (tokenised)
        alt Payment Failed
            PG-->>B: Payment declined (reason)
            B-->>F: Payment error
            F-->>C: Display payment error
        end
    end

    PG-->>B: Authorisation confirmed
    B->>DB: Create order & send stock update
    DB-->>B: Order indexed & stock decremented
    B-->>F: Order confirmation + receipt
    F-->>C: Display confirmation
```