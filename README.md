# Interview_Cake: Trading Apple Stock
I wanna know how much money I could have made yesterday if I'd been trading Apple stocks all day.

So I grabbed Apple's stock prices from yesterday and put them in an array called stockPrices, where:

The indices are the time (in minutes) past trade opening time, which was 9:30am local time.
The values are the price (in US dollars) of one share of Apple stock at that time.
So if the stock cost $500 at 10:30am, that means stockPrices[60] = 500.

## Purpose
Write an efficient method that takes stockPrices and returns the best profit I could have made from one purchase and one sale of one share of Apple stock yesterday.

For example:

  int[] stockPrices = new int[] {10, 7, 5, 8, 11, 9};

  getMaxProfit(stockPrices);
  // returns 6 (buying for $5 and selling for $11)


No "shorting"—you need to buy before you can sell. Also, you can't buy and sell in the same time step—at least 1 minute has to pass.

### Gotchas
You can't just take the difference between the highest price and the lowest price, because the highest price might come before the lowest price. And you have to buy before you can sell.

What if the price goes down all day? In that case, the best profit will be negative.

You can do this in O(n)O(n) time and O(1)O(1) space!

## Solution
We’ll greedily ↴ walk through the list to track the max profit and lowest price so far.

For every price, we check if:

we can get a better profit by buying at min_price and selling at the current_price
we have a new min_price
To start, we initialize:

min_price as the first price of the day
max_profit as the first profit we could get
We decided to return a negative profit if the price decreases all day and we can't make any money. We could have raised an exception instead, but returning the negative profit is cleaner, makes our function less opinionated, and ensures we don't lose information.

   
        def get_max_profit(stock_prices):
          if len(stock_prices) < 2:
              raise ValueError('Getting a profit requires at least 2 prices')


      # We'll greedily update min_price and max_profit, so we initialize
      # them to the first price and the first possible profit
          min_price = stock_prices[0]
          max_profit = stock_prices[1] - stock_prices[0]

      # Start at the second (index 1) time
      # We can't sell at the first time, since we must buy first,
      # and we can't buy and sell at the same time!
      # If we started at index 0, we'd try to buy *and* sell at time 0.
      # This would give a profit of 0, which is a problem if our
      # max_profit is supposed to be *negative*--we'd return 0.
          for current_time in range(1, len(stock_prices)):
              current_price = stock_prices[current_time]

          # See what our profit would be if we bought at the
          # min price and sold at the current price
          potential_profit = current_price - min_price

          # Update max_profit if we can do better
          max_profit = max(max_profit, potential_profit)

          # Update min_price so it's always
          # the lowest price we've seen so far
          min_price = min(min_price, current_price)

          return max_profit
    

## Complexity
O(n)O(n) time and O(1)O(1) space. ↴ We only loop through the list once.

# What We Learned
This one's a good example of the greedy ↴ approach in action. Greedy approaches are great because they're fast (usually just one pass through the input). But they don't work for every problem.

How do you know if a problem will lend itself to a greedy approach? Best bet is to try it out and see if it works. Trying out a greedy approach should be one of the first ways you try to break down a new question.

To try it on a new problem, start by asking yourself:

"Suppose we could come up with the answer in one pass through the input, by simply updating the 'best answer so far' as we went. What additional values would we need to keep updated as we looked at each item in our input, in order to be able to update the 'best answer so far' in constant time?"

In this case:

The "best answer so far" is, of course, the max profit that we can get based on the prices we've seen so far.

The "additional value" is the minimum price we've seen so far. If we keep that updated, we can use it to calculate the new max profit so far in constant time. The max profit is the larger of:

The previous max profit
The max profit we can get by selling now (the current price minus the minimum price seen so far)
Try applying this greedy methodology to future questions.
