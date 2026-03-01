# Game Instance

* game config
* LogDiagnostics

## Dedicated Server

* server check
* create eos session

#### Create EOS Session

<figure><img src=".gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

* get firestore game state document and modifiers
* get data

**Session Filters**

<figure><img src=".gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

* upload failure logs to s3
* put log in cloudwatch
* host beacon
  * set beacon port
* shutdown if couldn't host session

## Player

<figure><img src=".gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

* settings from gameusersettings
* load custom player settings from saved struct

### Login

<figure><img src=".gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

* set as guest if failed login
* check user exists in dynamo

### Sign Up

* create player in dynamo

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

* retry or continue as guest

## Query Process

<figure><img src=".gitbook/assets/UnrealEditor_DekRFnhEmA.gif" alt=""><figcaption></figcaption></figure>

* join with party from previous session dedicated server
* set and recalibrate steam purchased items
* get player doc from dynamo

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

* add default player items
* open tutorial for new player

#### Challenges

* get challenges from dynamo
* app sync update challenges daily & weekly
* **subscribe to challenges dynamo**

<figure><img src=".gitbook/assets/image (30).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

* get friends from steam
* get friend level and equipped items from dynamo

<figure><img src=".gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

* verify player loadouts match rules & server
* check and claim rewards
* set steam achievements
* set analytics

<figure><img src=".gitbook/assets/UnrealEditor_geutt7yEUn.gif" alt=""><figcaption></figcaption></figure>

