## Class Diagram for Nothing Store
### Source: Unit 3 Assessment

This diagram was still designed to fit the brief of user product search + placing an order.
However, at the time, for the assessment brief, I chose to omit user account functionality. This would be a feature that would be implemented in practice however.

#### Diagram

```mermaid
classDiagram
    class Customer {
        +int CustomerId
        +string CustomerName
        +string CustomerEmail
        +string CustomerDeliveryAddress
        +bool CustomerIsKnown
    }

    class Cart {
        +int CartId
        +float CartPriceTotal
        +AddItemToCart(Product, int quantity)
        +RemoveItemFromCart(Product)
        +ClearCart()
        +GetTotalPriceOfCart() float
    }

    class CartItem {
        +int QuantityOfItemInCart
        +float ItemInCartPriceTotal
    }

    class Product {
        +int ProductId
        +string ProductName
        +string ProductDescription
        +string ProductInformation
        +float ProductItemRating
        +float ProductPrice
        +string ProductImageUrl
        +bool ProductIsAvailable
    }

    class Category {
        +int CategoryId
        +string CategoryName
    }

    class Stock {
        +int ItemId
        +int ItemInStockQuantity
        +ReserveInCarts(int qty) bool
        +DecrementStock(int qty)
        +ReleaseReservation(int qty)
    }

    class Order {
        +int OrderId
        +DateTime OrderCreatedDate
        +string OrderStatus
        +float OrderPriceTotal
        +string CustomerDeliveryAddress
        +PlaceOrder()
        +CancelOrder()
    }

    class OrderItem {
        +int QuantityOfItemInOrder
        +float UnitPricePerItem
        +float PriceOfAllItems
    }

    class Payment {
        +int PaymentId
        +float AmountToPay
        +string PaymentMethod
        +string PaymentStatus
        +DateTime PaymentProcessedDateAndTime
        +AuthorisePayment() bool
        +RetryPayment() bool
        +RefundPayment() bool
    }

    class RecommendationEngine {
        +GetPersonalisedRecommendations(Customer) List~Product~
        +GetPopularItems() List~Product~
    }

    Customer "1" --> "1" Cart : has
    Cart "1" --> "*" CartItem : contains
    CartItem "*" --> "1" Product : references
    Product "*" --> "1" Category : belongs to
    Product "1" --> "1" Stock : tracked by
    Customer "1" --> "*" Order : places
    Order "1" --> "*" OrderItem : contains
    OrderItem "*" --> "1" Product : references
    Order "1" --> "1" Payment : paid via
    RecommendationEngine --> Customer : analyses history of
    RecommendationEngine --> Product : suggests
```