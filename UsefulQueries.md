```sql
select comboName, poorDesign.price as comboPrice, inventory.item as item, prices.price as itemPrice
    from poorDesign 
    cross join inventory on poorDesign.item = inventory.id 
    cross join prices on poorDesign.item = prices.id;
```
