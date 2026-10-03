# Reflection

I used Antigravity in place of GitHub Copilot because my student verification failed. Five of my six measures were usable as suggested: Total Sales, MoM Growth %, Running Total, City Rank and Cold Brew Sales. They used the right table and column names, and the scorecard's final Running Total matched the Total Sales card.

The one real correction was Item Share of Category %. Antigravity used `ALL(Dim_Product)` in the denominator, which clears the Category filter along with Item, so each item would show its share of all sales instead of its category. I changed it to `ALL(Dim_Product[Item])`, which keeps the Category filter. This only works with Category in the visual's context, so my chart is filtered to Coffee.

Antigravity's README draft also needed fixing. It invented a "Cafe" store format, said sales grew from April to June when they rose and then fell, and mentioned a beverage category that doesn't exist. I caught these by checking it against my dashboard.

Committing one measure at a time made me finish and check each measure before starting the next, and the history now shows the Item Share correction clearly. My main lesson is that AI output is a starting point that I must verify against my own data.