---
title: 'Rating Algorithm in chess'
author: 'Harpreet Singh'
math: true
---
# Chess rating system
## Introduction
A chess rating system is a system used in chess to estimate the strength of a player, based on their performance versus other players. They are used by organisations like FIDE, English Chess Federation and online chess websites like chess.com, Lichess only to name a few. In general for a player, their strength is determined by the rating, higher rating indicates more proficiency.

Winning against higher-rated players boosts one's rating, while losing to lower-rated players leads to a decrease. But how exactly do we give the rating as in attempt to consolidate all the factors into a single number, we are likely to miss some aspects. We therefore try to make it as fair as it should. That's why the most commonly used rating is the `Elo rating system` invented by Arpad Elo. USCF uses USCF system in which `K factor` is varies for each match and player is given bonus points for the superior performance in the tournament.
So let's first understand what is the Elo rating system in an easy way.

## The Elo Rating System
### Simple Example
First we need to know what is the `expected score` and what is an `actual score` of the player in a match. For instance, a player1 of rating 1200 plays 16 matches and wins 10 , and draws 1,against player2 with rating 1000, his actual score is 10.5(1 for each win, 0.5 for each draw, 0 for a loss :/), however expected score is calculated through the probability of the winning which is
$$P1 = \frac{1.0}{1.0 + 10^{\frac{{\text{{rating1}} - \text{{rating2}}}}{400}}}$$
Rating difference of  gave player1 expected score of 0.75. This is an impossible score and a draw would be costly conversely a draw would improve the rating of player2.
The formula is 

$$
\begin{align*}
NewRating &= OldRating + K \cdot (actual - expected) \tag{i} \\
\end{align*}
$$
where $K$=10


### Linear Approximation
Elo devised a linear approximation to his full system, negating the need to calculate expected score. So we have 

$$ R_{\text{new}} = R_{\text{old}} + \frac{K(W_L)}{2} - \frac{K}{4C} \sum_{i} D_i $$


Where $R_{\text{new}}$ and $R_{\text{old}}$ are `new` and `old` ratings respectively, $D_i$ is the difference of opponents' rating minus the player's rating, $W$ is the number of wins, and $L$ is the number of losses, $C=200$, and $K=32$. The term $\frac{W-L}{2}$ is the score above or below 0, and $\frac{\sum D}{4C}$ is the expected score according to: 4C rating points equals 100%. Note that this of the similar form as $(i)$.

The USCF used a modification of this system to calculate ratings after individual games of [correspondence chess](https://en.wikipedia.org/wiki/Correspondence_chess)(it is usually played through a correspondence chess server, a public internet chess forum, or email, in contrast to over the board chess) with a $K$ = 32 and $C$ = 200. 

## Code
Note that this C++ code only implements the equation $(i)$
```c++

#include <bits/stdc++.h>
using namespace std;


float Probability(int rating1, int rating2)
{
	return 1.0 * 1.0/(1+ 1.0* pow(10,1.0 * (rating1 - rating2) / 400));
}

void EloRating(float Ra, float Rb, int K, int d)
{

	// To calculate the Winning Probabilities
	float Pb = Probability(Ra, Rb);
	float Pa = Probability(Rb, Ra);

    // Case When Player A wins
	if (d == 1) {
		Ra = Ra + K * (1 - Pa);
		Rb = Rb + K * (0 - Pb);
	}
	// Case When Player B wins
	else if(d==0){
		Ra = Ra + K * (0 - Pa);
		Rb = Rb + K * (1 - Pb);
	}
    // Case of draw
    else if (d==0.5){
        Ra = Ra + K * (0.5- Pa);
		Rb = Rb + K * (0.5 - Pb);
    }

	cout << "Updated Ratings:\n";
	cout << "Ra = " << Ra << " Rb = " << Rb<<endl;
}


int main()
{

	float Ra , Rb;
    cout << "Enter Ra and Rb: ";
        if (!(cin >> Ra >> Rb) || !is_floating_point<decltype(Ra)>::value || !is_floating_point<decltype(Rb)>::value) {
        std::cerr << "Error: Ra and Rb must be floats." << std::endl;
        return 1; 
    }


	int K = 10;
    int d;
    cout<<"Enter the result"<<endl;
    cin>>d;


	EloRating(Ra, Rb, K, d);
    cout<<endl;
	return 0;
}

```
Credits: [Wikipedia](), [GFG](https://www.geeksforgeeks.org/elo-rating-algorithm/), [Medium](https://stanislav-stankovic.medium.com/elo-rating-system-6196cc59941e)

Thanks for reading!🤓