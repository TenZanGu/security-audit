# security-audit

## Task
Perform a security audit on the contract. Document in a report all the issues/vulnerabilities, classify their severities and provide recommendations.

## Disclamer!
This project is used for practicing security audit on a contract intentially coded with security vunerabilities.

Audited by : Lobsang Tenzin 

student Id : 101247081

## Contract Audited
BadKickback.sol 

link: https://github.com/TenZanGu/security-audit/blob/master/contracts/BadKickback.sol

The purpose of the contract, BadKickback, is used to encourage higher event turnout rate. The rule is to charge everyone a small fee when they sign up for the event. The fee will be refunded after the event check-in. No-shows will lose their fee, which will be split amongst the attendees.

## Issues/Vulnerabilities
In total, 5 issues were reported including:

#### 1. owner privileges (the ability of an owner to manipulate contract, may be risky for participants).

#### severity: critical

#### Description
only the owner can payout().

#### Recommendation
Don't let the owner control the function.
remove this code require(owner == msg.sender, "only owner can payout");

#### 2. owner can choose who to payout.

#### severity: critical

#### Description
the owner can choose the attendees for the payout().

#### Recommendation
make the contract payout to all participents.
remove this code (address[] memory attendees) and in the payout function replace all of the attendees with participants that was used to hold the users that payed for the event. mapping(address=>bool) public participants;

#### 3. Re-entrancy.

#### severity: critical

#### Description
the address in attendees can be a contract address with a fallback and do a delegatecall.

#### Recommendation
remove this code (address[] memory attendees) and in the payout function replace all of the attendees with mapping(address=>bool) private attended and create a attended() function with require(owner == msg.sender, "only owner can mark attendance") into a mapping(address=>bool) private attended;

#### 4. owner gets all the ether.

#### severity: critical

#### Description
the owner can destroy() and get all the ether.

#### Recommendation
make the destroy function just destroy the contract.
replace this code selfdestruct(owner) with selfdestruct().

#### 5. public function

#### severity: critical 

#### Description
Anyone can call destroy().

#### Recommendation
add this code require(owner == msg.sender, "only owner can destroy");

#### 6. Not reaching Cap and stopping the participants.

#### severity: low

#### Description
the participantCount is not equal to cap in require in participate().

#### Recommendation
add and = in the code after < require(participantCount <= cap, "this event is closed");


#### 7. Not reaching Cap and stopping the payout.

#### severity: low

#### Description
the participantCount is not equal to cap in require in payout().

#### Recommendation
add and = in the code after < require(participantCount <= cap, "this event is closed");

## Conclusion
The audited smart contract can not be deployed. This contract is not trustable. Many critical severity were found which shows that this contract may be to scam ether from participants.