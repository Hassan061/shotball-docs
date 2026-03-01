# Base User Widget

* begin query process
* set up subscription for dynamo friend invites
* initialize challenges widget



## Session Finding



<figure><img src=".gitbook/assets/UnrealEditor_27xNVNZzAD.gif" alt=""><figcaption></figcaption></figure>

* join reserved competitive match if left
  * store match id for leaver to get back to it

<figure><img src=".gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

* bot mode: join right away without session

<figure><img src=".gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

#### Find AWS Session

* if AWS: find AWS Session first

<figure><img src=".gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

* If session fails, we re-search again&#x20;

<figure><img src=".gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

* Create AWS Game Lift Session

<figure><img src=".gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

* Repeat search on every session fail

#### Filters:

* **Game Mode** equals Server
* **IsCanJoin** set by Server
* **NumberOfPlayers** less than or equals Highest Number we can accommodate
  * Aim for sessions with the most players but still we can enter them (3 or 2 or 1 slots available based on party)
* **MaxPartySize** greater than or equals highest our party size
  * So in 3v3: max party size is 3 people in one team. 2v2: max party size is 2 people in one team
* **Region** equals the one we set with the Server
  * In AWS first session, we want to create it in the region we select rather than a group of them

<figure><img src=".gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

* Party Averag MMR is between **LowestMMR** and **HighestMMR** calculated by the server&#x20;
* Ranked not equals **OnlyReserveJoin**

<figure><img src=".gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>



<figure><img src=".gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

#### Find EOS Session

* we find session again but with additional **Regions** if a lot of time has passed searching

<figure><img src=".gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

* Get all sessions found and sort them by **Timestamp** to join latest valid sessions first



<figure><img src=".gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

* Set SessionUpdateProperties, which are info to provide to the server for Matchmaking
  * **User ID, MMR**

#### Connect to Beacon

<figure><img src=".gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

* find the **ServerBeaconPort** to connect to



<figure><img src=".gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

* Get Heartbeat from Server, otherwise disconnect&#x20;

#### Join Session

<figure><img src=".gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

* Join Session when received signal from Server's beacon
* Let Party members join first, then the party leader joins
* On Logged out indicates the party member has joined the session

<figure><img src=".gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

* EOS: just Join Session

<figure><img src=".gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

* AWS: Update GameLift Session then join by IP & Port

<figure><img src=".gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

* Parties join by broadcasted Session ID

