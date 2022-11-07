# API Design

### Participants:
 - Customer
 - Store Clerk
 - Support

### Activities:
1. Customer searches for an Item
2. Customer adds Item to their cart
3. Customer adds and removes more Items
4. Customer logs into Customer Account
5. Customer checks out
6. Store Clerk fulfills Order
7. a. If an Item was not found, Support contacts Customer
8. b. If all Items were found, Store Clerk ships Order

### Objects:
 - Item
 - Cart
 - Customer Account
 - Order

### Resources:
- Search items              GET
- View item                 GET
- Add first item to cart    POST
- Add and remove items      PUT
- View cart                 GET
- Check out                 POST
- List orders               GET
- View order                GET
- Cancel order              POST/PUT (not DELETE)

### Dependencies:
 - Items are independent
 - Carts are created for Items (dependent)
 - Orders come from Carts (dependent)
 - Orders are placed by Customer Accounts (dependent)
 - Customer Accounts are independent