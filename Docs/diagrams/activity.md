## Activity Diagram for Customer Product Search + Order
### Source: Unit 3 Assessment

For this, I had chosen to use PlantUML to render the diagram.
Unfortunately, unlike Mermaid, PlantUML is not rendered automatically in GitHub preview environments, so an additional image is attached.

This diagram follows the user in:
- Finding a product listing (either search or recommendation by algo)
- Placing product in cart
- Checking out cart and placing an order

The diagram further shows which part of the system is responsible for which functionality (e.g. Backend holds product info and stock count etc)

#### Diagram

![Nothing Store Activity Diagram, showing flowchart from user item selection to placing an order](<Activity Diagram.png>)

#### Diagram Code

```
@startuml

|Customer|
start
:Visit Website;

repeat :Browse / Search Products;

|Frontend|
:Display Product Listings;
if (Known User?) then (Yes)
  :Show Personalised Recommendations
  (based on account or digital footprint);
else (No)
  :Show Popular / Trending Items;
endif

|Customer|
:Select a Product;

|Backend|
:Request Product Details & Stock Level;

|Database|
:Return Product Data & Stock Quantity;

|Backend|
if (In Stock?) then (Yes)
  :Return Product Info to Frontend;
  |Frontend|
  :Display Product Page
  (name, price, images, availability, information);
  |Customer|
  :Add Item to Cart;
  |Frontend|
 :Update Cart State & Total;
else (No)
|Frontend|
:Display "Out of Stock" Message;
endif

|Customer|
backward :Return to Browse;
repeat while (Continue Shopping?) is (Yes) not (No)

:Proceed to Checkout;

|Frontend|
:Display Checkout Form
(delivery address, payment details);

|Customer|
repeat :Enter Delivery & Payment Details;
|Frontend|
backward :Display Validation Errors;
repeat while (Input Valid?) is (No) not (Yes)

:Submit Order to Backend;

|Backend|
:Validate & Sanitise Input (server-side);
:Encrypt Payment Data (TLS/tokenisation);
:Send Payment Request;

|Payment Gateway|
repeat :Authorise Payment;
backward :Display Payment Error;
repeat while (Payment Authorised?) is (No) not (Yes)

:Return Success Token;

|Backend|
:Create complete Order & Send Stock Update;

|Database|
:Index Order & Decrement Stock;

|Backend|
:Return Order Confirmation;

|Frontend|
:Display Order Confirmation & Receipt;

|Customer|
:Receive Confirmation;
stop

@enduml```