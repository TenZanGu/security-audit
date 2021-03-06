# security-audit

## Task
Perform a security audit on the contract. Document in a report all the issues/vulnerabilities, classify their severities and provide recommendations.

## Disclamer!
This project is used for practicing security audit on a contract intentially coded with security vunerabilities and this audit is not guarantee the contract is bug free.

Audited by : Lobsang Tenzin 

student Id : 101247081

## Contract Audited
BadKickback.sol 

link: https://github.com/TenZanGu/security-audit/blob/master/contracts/BadKickback.sol

The purpose of the contract, BadKickback, is used to encourage higher event turnout rate. The rule is to charge everyone a small fee when they sign up for the event. The fee will be refunded after the event check-in. No-shows will lose their fee, which will be split amongst the attendees.

## Issues/Vulnerabilities
In total, 6 issues were reported including:

* 2 low severity issues.

* 1 high severity issue.

* 2 critical severity issues.

* 1 owner privileges (the ability of an owner to manipulate contract, may be risky for participants)

#### 1. owner can choose who to payout.

#### severity: high

#### Description
the owner can choose the attendees for the payout().

#### Recommendation
make the contract payout to all attended.
remove this code (address[] memory attendees) and in the payout function replace all of the attendees with attended that is used to hold the users that attended the event. mapping(address=>bool) public attended; this can be check by making a ticket which is scaned at the event.

#### 2. Re-entrancy.

#### severity: critical

#### Description
the participants address can be a contract address with a fallback and do a delegatecall.

#### Recommendation
set a withdrawal limit for each address after counting the amount for each address attended.

#### 3. owner gets all the ether.

#### severity: owner privileges (the ability of an owner to manipulate contract, may be risky for participants)

#### Description
the owner can destroy() and get all the ether.

#### Recommendation
make the destroy function just destroy the contract.
replace this code selfdestruct(owner) with selfdestruct().

#### 4. public function

#### severity: critical 

#### Description
Anyone can call destroy().

#### Recommendation
add this code require(owner == msg.sender, "only owner can destroy");

#### 5. Not reaching Cap and stopping the participants.

#### severity: low

#### Description
the participantCount is not equal to cap in require in participate() so there can only be 49 participants.

#### Recommendation
add and = in the code after < require(participantCount <= cap, "this event is closed");


#### 6. Not reaching Cap and stopping the payout.

#### severity: low

#### Description
the participantCount is not equal to cap in require in payout().

#### Recommendation
add and = in the code after < require(participantCount <= cap, "this event is closed");

## Conclusion
The audited smart contract can not be deployed. This contract is not trustable. Many critical severity were found. Warning this contract may be to scam ether from participants.