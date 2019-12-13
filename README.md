# Team-Project-Fall19-CIS-7

// Welcome to the Game of Blackjack, which the game was coded by the Pseudo Boomers. The player is against the dealer, and dealt two cards, as the dealer's first care is face down.
//
// We took inspriation from other coded blackjack games already made, and used what we saw and made the game code from there. 
//
#include <algorithm>
#include <cstdlib>
#include <ctime>
#include <iostream>
#include <string>
#include <vector>
using namespace std;

//Code Starts

class C {
//giving variables names
private:
	int value;
	string suit;
	bool isAce;
	bool isFaceCard;
//variable naming 
public:
	C();
	C(int, string, bool, bool);
	void displayCardInfo();
	void displayPlayerInfo();
	void displayDealerInfo();
	int getValue();
	string getSuit();
	bool getisAce();
	bool getisFaceCard();
	int getCardValue(int total);
};

int C::getValue() {
	return value;

}
string C::getSuit() {
	return suit;

}
bool C::getisAce() {
	return isAce;

}
bool C::getisFaceCard() {
	return isFaceCard;

}
//Card values and when 0 is empty
C::C() {
	value = 0;
	suit = "Empty";
	isAce = false;
	isFaceCard = false;
}

C::C(int v, string s, bool a, bool f) {
	/*this->id = id;
		this->capacity = capacity;
		this->reserved = reserved;
	*/
	this->value = v;
	this->suit = s;
	this->isAce = a;
	this->isFaceCard = f;
}
//Proccessing values.
void C::displayPlayerInfo() {
	string CNumTemp = std::to_string(value);
	if (value == 1) {
		CNumTemp = "A";
	} else if (value == 11) {
		CNumTemp = "J";
	} else if (value == 12) {
		CNumTemp = "Q";
	} else if (value == 13) {
		CNumTemp = "K";
	}
//
	cout << "Player dealt " << CNumTemp << " Of " << suit << endl;
  //Describes the result with the player and cards selected.
}

void C::displayDealerInfo() {
	string CNumTemp = std::to_string(value);
	if (value == 1) {
		CNumTemp = "Ace";
	} else if (value == 11) {
		CNumTemp = "Jack";
	} else if (value == 12) {
		CNumTemp = "Queen";
	} else if (value == 13) {
		CNumTemp = "King";
	}

	cout << "Dealer dealt " << CNumTemp << " Of " << suit << endl;
}

void C::displayCardInfo() {
	string CNumTemp = std::to_string(value);
	if (value == 1) {
		CNumTemp = "Ace";
	} else if (value == 11) {
		CNumTemp = "Jack";
	} else if (value == 12) {
		CNumTemp = "Queen";
	} else if (value == 13) {
		CNumTemp = "King";
	}

	cout << CNumTemp << " of " << suit << endl;
}

int C::getCardValue(int total) {
	if (isAce) {
		if (total >= 11) {
			return 1;
		}
		return 11;
	} else if (isFaceCard) {
		return 10;
	}
	return value;
}

int main() {
	  //while gameOver = false; keep going
	vector<C *> playerHand;
	vector<C *> dealerHand;
	cout << "Welcome to Blackjack!" << endl;
	vector<C *> Deck;

	vector<C *>::iterator y;
	// for(i = 0 ; i < Storage.size(); i++){
	//   if (*y != storage[i]->get_name())
	// cout << "Person  found;

	for (int suitnum = 0; suitnum <= 3; suitnum++){
		for (int cnum = 1; cnum <= 13; cnum++) {
			string suitTemp;
			bool isFaceCardTemp = false;
			bool isAceTemp = false;
			if (suitnum == 0) {
				suitTemp = "CLUBS";
			}
			if (suitnum == 1) {
				suitTemp = "HEARTS";
			}
			if (suitnum == 2) {
				suitTemp = "SPADES";
			}
			if (suitnum == 3) {
				suitTemp = "DIAMONDS";
			}

			if (cnum == 1) {
				isAceTemp = true;
      			}
			if (cnum >= 11 && cnum <= 13) {
				isFaceCardTemp = true;
			}
			Deck.push_back(
				new C(cnum, suitTemp, isAceTemp, isFaceCardTemp));
		}
	}

	// for (y = Deck.begin(); y != Deck.end(); y++) {
	//	(*y)->displayCardInfo();
	//}

	srand(unsigned(time(0)));
	random_shuffle(Deck.begin(), Deck.end());

	int playerTotal = 0;
	int dealerTotal = 0;
	int dealtCIndex = 0;
	playerTotal = playerTotal + Deck[dealtCIndex]->getCardValue(playerTotal);
	Deck[dealtCIndex]->displayPlayerInfo();
	dealtCIndex++;
	playerTotal = playerTotal + Deck[dealtCIndex]->getCardValue(playerTotal);
	Deck[dealtCIndex]->displayPlayerInfo();
	cout << "Player score: " << playerTotal << endl;
	dealtCIndex++;

	string hit;
	cout << "Hit or stand (H/S): " << endl;
	cin >> hit;

	while (playerTotal < 22 && (hit == "h" || hit == "H")) {
		playerTotal =
			playerTotal + Deck[dealtCIndex]->getCardValue(playerTotal);
		Deck[dealtCIndex]->displayPlayerInfo();
		cout << "Player score: " << playerTotal << endl;
		if (playerTotal < 22) {
			cout << "Hit or stand (H/S): " << endl;
			cin >> hit;
		}
		dealtCIndex++;
	}
	cout << "Player score: " << playerTotal << endl;
	if (playerTotal > 21) {
		cout << "Bust!  Game over..." << endl;
		return 0;
	
	dealerTotal = dealerTotal + Deck[dealtCIndex]->getCardValue(dealerTotal);
	Deck[dealtCIndex]->displayDealerInfo();
	dealtCIndex++;
	dealerTotal = dealerTotal + Deck[dealtCIndex]->getCardValue(dealerTotal);
	Deck[dealtCIndex]->displayDealerInfo();
	dealtCIndex++;

//ending whether busted or won.
	while (dealerTotal < 17 && dealerTotal < 21) {
		dealerTotal =
			dealerTotal + Deck[dealtCIndex]->getCardValue(dealerTotal);
		Deck[dealtCIndex]->displayDealerInfo();
		dealtCIndex++;
	}
	cout << "Dealer score: " << dealerTotal << endl;
	if (dealerTotal > 21) {
		cout << "Bust! You win!" << endl;
	}
	if (playerTotal >= dealerTotal) {
		cout << "Dealer stands." << endl;
		cout << "You win!" << endl;
	} 
  else {
		cout << "Dealer wins." << endl;
	}
}
//________________________________________INSTRUCTIONS_____________________________________

//
//Blackjack is a card game between a player and the dealer using a deck of 52
//cards. The object of the game is to reach a score as close to 21 as possible
//without exceeding it (bust). Both the dealer and the player are initially dealt
//two cards and have the option to receive another card (hit) or keep their
//current score (stand). The player can continually hit until they score 21 or
//bust. A dealer is required to stand on anything inclusively between 17 and 21
//A score is calculated by adding up the values of the cards in a player's hand.
//Face cards (Jack, Queen, King) are worth 10 points and Aces are worth either 1
//point or 11 points (whichever value yields the best score).
//The player is dealt first and continues to play until they bust or stand. Once a
//player stands, the dealer will then play until they bust or stand. If a player
//busts, the dealer wins. If the dealer busts, the player wins. If neither busts,
//whoever has the score closest to 21 wins. In case of a tie, neither the player
//nor the dealer wins.

}
