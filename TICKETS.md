# QA has submitted the following tickets

## Filtering Orders
### QA Notes
When getting all orders filtered by a property, the orders are not being filtered at all. I tried filtering the orders by name for any order that was an "Additional Topping" but I'm getting all orders back.

### Tips
For query params you will want to assume `filterProperty` is "name" and `filterValue` is "Additional Topping".

### Dev Notes / Response
Root cause: the ordersData array was being initially filtered when it should have been mapped. And since when the outer filter receives a returned filter from the inner filter, it will end up passing back every single result unfiltered.

Solution: have orderData be mapped instead of filtered

---


## Placing An Order
### QA Notes
When testing an order for a family of 6, the total is not as expected. I placed an order for the following: 

    - 2 Cheeseburgers
    - 2 Pickle Toppings
    - 1 Large Fiesta Salad
    - 3 Avocado Toppings
    - 1 Medium Hawaiian Pizza
    - 3 Medium French Fries
    - 4 Large Fountain Drinks

I calculated that the total should be $74.23 but I'm getting $51.28. Because that's a difference of $22.95, I have a feeling the "Medium Hawaiian Pizza" isn't being added.

### Tips
All items ordered (and more) can be referenced in lib/orders.js

### Dev Notes / Response
Root Cause: The issue was the item quantity was not being accounted for in the total calulations

Solution: multiply the item price by the quantity

Note: The "Medium Hawaiin Pizza" red herring was a misunderstanding. This is because the total number of item prices, without multiplying the quantity, is $51.28. And the total number of item prices WHILE multiplying the quantity but NOT including the hawaiin pizza is also $51.28. Very confusing but just a coincidence.

---


## Updating An Order
### QA Notes
When getting updating an order I expect to only have to pass what has changed. However, if I don't pass everything (customerName or items), that value gets removed. If for instance I did not change the customer name, I would expect it to use the one originally on the order.

Additionally, when updating the items ordered, the total is not updating.

### Dev Notes / Response
Root Cause: customerName and items were being returned without checking if they were not undefined. The total was also not updated when there are new items avaible to be updated.

Solution: Conditionally add customerName, items, and total to the updated object.

---


## Deleting An Order
### QA Notes
When  I delete an order, the order that gets deleted is never the one I expect. I know we recently changed how we are doing our deletes so I'm not sure everything got updated. But when I delete a specific order, that's usually not the one that gets deleted. Unless I delete it immediately.

### Dev Notes / Response
Root Cause: The filter within delete was trying to access the id of an undefined variable

Solution: Use the found order to be deleted as the proper filter

---


## Other

## Adding orders
### QA Notes
When I add an order, I am allowed to add anything to the orders array.

For example Ella Vader has the field price when all other customers have theirs as total

### Dev Notes / Response
A schema should be added to account for this

