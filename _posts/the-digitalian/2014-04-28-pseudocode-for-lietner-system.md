---
layout: post
categories: 
- digitalian
- notes
comments: true
title: Pseudocode for Lietner System
image:
  feature: birch-trees2.jpg
  credit: Francesco Gallarotti
  creditlink: http://www.gallarotti.net
---
This is just a note to myself on how to implement the main [Lietner System](http://leitnerportal.com/LearnMore.aspx) algorithm.
First, this is the pseudocode for the main loop to review everything that needs to be reviewed:

```
foreach(Deck in User.Decks) {
	foreach(Card in Deck.Cards) {
		if((time.Now()-Card.LastReview) > Deck.TimeToReview {
			Card.Review();
		}
	}
}
```

Next is the single card review method:

```
if(review result is correct) {
	// if the card is in the last deck, increase the card memorization
	// level by one and put it back in deck 1 
	if(Card.Deck.Index = Decks.LastDeckIndex) {
		Card.Level++
		if(Card.Level < Card.Deck.MaxLevel) {
			// restart from deck 1 for next level review
			Card.Deck.Index = 1
		}
		else {
			// card has been memorized successfully 
			// remove card from deck 
		}
	}
	else {
		card.Deck.Index++
	}
}
else {
	card.Deck.Index = 1
	card.Level = 0
}
```
