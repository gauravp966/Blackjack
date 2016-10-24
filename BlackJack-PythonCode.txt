import random
cardNumber = 0
cardColor = 0
playerPoints = 0
dealerPoints = 0
totalMoney = 0
betAmount = 0
cardPoints = 0
aCard = 0
playerCall = 'hit'
playerCards = []
dealerCards = []
playerPlaying = True
gameOn = False
dealer2ndChance = False
playingAgain = True
bothStay = False

def deal_a_card():
    global cardNumber, cardColor, cardPoints, aCard
    spades = u'\u2660'
    hearts = u'\u2665'
    diamonds = u'\u2666'
    clubs = u'\u2663'
    cardRange = ['A', 2, 3, 4, 5, 6, 8, 9, 10, 'J', 'Q', 'K']
    cardNumber = random.choice(cardRange)
    
    if cardNumber == 'J' or cardNumber == 'Q' or cardNumber == 'K':
        cardPoints = 10
    elif cardNumber == 'A':
        cardPoints = 11
    else:
        cardPoints = cardNumber
        
    allColors = [spades,hearts,diamonds,clubs]
    cardColor = random.choice(allColors)
    aCard = [cardNumber,cardColor]
    print aCard[0], aCard[1]
    
def player():
    
    global totalMoney
    
    playerCards = []
    
    name = raw_input("Your name: ")
    while True:
        try:
           
            totalMoney = int(raw_input("Total amount of money you have: "))
        except:
            print "Please enter a valid amount!"
            continue
        else:
            print "Your total amount is $",totalMoney
            print "Lets play!"
            break
    
    
            
def dealer():
    print "Hi, I am the dealer."
            
def bet():
    global betAmount, gameOn
    while True:
        try:
            betAmount = int(raw_input("\nHow much money do you wanna bet? "))
        except:
            print "Please enter a valid amount!"
            continue
        else:
            print "Your bet is $",betAmount
            break
    gameOn = True

def firstPlay():
    global dealerPoints, playerPoints, cardPoints, aCard, dealer2ndChance
    print "\nOkay, let's play!"
    print "\nDealer:"
    deal_a_card()
    #dealerCards.append(aCard)
    dealerPoints += cardPoints
    print 'Dealer points: ',dealerPoints
    
    print "\nPlayer:"
    deal_a_card()
    playerCards.append(aCard)
    playerPoints += cardPoints
    deal_a_card()
    playerCards.append(aCard)
    playerPoints += cardPoints
    print 'Player points: ',playerPoints
    
    dealer2ndChance = True
    
    pass


class RestPlay(object):
    global playerPoints, dealerPoints, cardPoints, dealerPlaying, playerPlaying, bothStay, gameOn
    
    def playerChance(self):
        global playerPoints, dealerPoints, cardPoints, playerPlaying
        playerCall = raw_input("Do you wanna hit or stay ? : ")
        
        while playerCall.lower() == 'hit' and playerPoints < 22:
            print "\nPlayer hits!"
            print "\nPlayer :"
            deal_a_card()
            playerPoints += cardPoints
            playerPlaying = False
            print 'Player points: ',playerPoints
            if playerPoints < 22:
                playerCall = raw_input("\nDo you wanna hit or stay ? : ")
           
        if playerCall.lower() == 'stay':
            playerPlaying = False
            print "\nPlayer stays!"
    
    
    def dealerChance(self):
        global playerPoints, dealerPoints, cardPoints, dealerPlaying, playerPlaying, bothStay, gameOn
        while dealerPoints < 17:
            print "\nDealer hits!"
            print "\nDealer: "
            deal_a_card()
            dealerPoints += cardPoints
            playerPlaying = True
            print "Dealer points: ",dealerPoints
            
        else:
            playerPlaying = True
            print "\nDealer stays!"
            bothStay = True
            gameOn = False
            

def winCheck():
    global totalMoney, betAmount, gameOn
   
    if playerPoints > 21:
        print "\nPlayer Burst! \n*** Dealer Wins! ***"
        totalMoney -= betAmount
        print "\nYour total amount now is $",totalMoney
        gameOn = False
        playerPlaying = True
        
    if dealerPoints > 21:
        print "\nDealer Burst! \n*** Player Wins! ***"
        totalMoney += betAmount
        print "\nYour total amount now is $",totalMoney
        gameOn = False
        playerPlaying = True
        
    if bothStay == True:
        if playerPoints > dealerPoints and playerPoints <= 21:
            print "*** Player wins! ***"
            totalMoney += betAmount
            print "\nYour total amount now is $",totalMoney
            gameOn = False
            playerPlaying = True
            
        elif dealerPoints > playerPoints and dealerPoints <= 21:
            print "*** Dealer wins! ***"
            totalMoney -= betAmount
            print "\nYour total amount now is $",totalMoney
            gameOn = False
            playerPlaying = True
            
        elif dealerPoints == playerPoints:
            print "*** DRAW! ***"
            print "\nYour total amount now is $",totalMoney
            gameOn = False
            playerPlaying = True
        
        
def playAgain():
    global playerPoints, dealerPoints, playingAgain, playerPlaying, totalMoney,bothStay
    playAgain = raw_input("Do you wanna play again? (yes or no) ")
    if playAgain.lower() == 'yes':
        playingAgain = True
        gameOn = True
        playerPlaying = True
        playerPoints = 0
        dealerPoints = 0
        bothStay = False
        
    elif playAgain.lower() == 'no':
        playingAgain = False
        print "\nYour total amount now: ",totalMoney
    
dealer()
player()

while playingAgain == True:
    bet()

    firstPlay()
    winCheck()

    while gameOn == True:
        while playerPlaying == True:
            RestPlay().playerChance()
            winCheck()
        while playerPlaying == False and gameOn == True:
            RestPlay().dealerChance()
            winCheck()
    playAgain()
       