# Beacon

<figure><img src=".gitbook/assets/UnrealEditor_fB2Am2TirG.gif" alt=""><figcaption></figcaption></figure>

* connect to Beacon by session or IP&#x20;
* Set the provided **SessionUpdateProperties**
* Once connected to the Server, it Filters the **SessionUpdateProperties** to pass or disconnect the player
  * We do this additional filtering because the client based filtering from the session is not up to date or reliable

<figure><img src=".gitbook/assets/UnrealEditor_cwNAplEIeQ.gif" alt=""><figcaption></figcaption></figure>

* Continue if:&#x20;
  * Match can be joined
  * Number of Players fitting for Game Mode
  * Party Size fitting for Game Mode
  * MMR of joined players fits threshold
  * Party can fit within the teams
    * We swap out solos to make room for parties, but parties need to stay together in one team
  * Player isn't a repeated leaver

#### Update SUPs

* Server updates its data when players pass
  * **NumberOfPlayers, MaxPartySize, HighestMMR, LowestMMR,**&#x20;

<figure><img src=".gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

#### Distribute Players in Teams

<figure><img src=".gitbook/assets/UnrealEditor_mmZZ5RDFtf.gif" alt=""><figcaption></figcaption></figure>

* Put Parties in the same Team
* Then, distribute Solos to balance MMR between the teams
* Before game starts, Solos are flexible in their team assignment so they could be swapped to make room for Parties

#### Update EOS Settings

* Server updates the Session to propagate back to new clients, so they could filter it properly

<figure><img src=".gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

* Increase MMR threshold as time goes on to allow more players to join if Session becomes stale

<figure><img src=".gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

* Start Match if reached max Players
  * Inform Clients to join Session
  * Disconnect Players if they took too much time to join

<figure><img src=".gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

* Beacon hands off to **Game State** in the Match level to track players joining
  * Allow players to join into the match level and disconnect the Beacons of Players who didn't make it in time
  *
