Faction Scripting Resources - An OWB Modder's Resource
======================================================
Standard techniques for tracking factions are not sufficient. It's common
practice to guess at whether or not a faction exists by looking for a
'canonical' faction leader, but this only works in the best case.

"But wtchappell, can't you just check who the faction leader is to see what
faction a country is in?"

No. If a country is in a faction, it is theoretically possible for them to be
that faction's leader.

"How?"

1. Faction leader declares an offensive war.
2. Other faction members choose not to get involved.
3. Faction leader loses the war.
4. A new faction leader is selected from the remaining members.
5. Now, if your code assumes that a faction exists only if the former leader
   exists and is a faction leader, it will break.

"Oh. Well, that's pretty specific."

It might be now, though it's entirely possible and does happen - especially
with smaller factions like the Nevada Pact or the Mojave Alliance.

"Well, it still doesn't happen very much."

It's going to start happening more when the next base game update allows an
actual process for becoming a faction leader without actually losing your
current faction leader first.

"OK, so how does this solve the problem?"

I'm solving this problem in a few ways. The most important of which is the
current_faction country variable. I've hooked multiple places in the code to
ensure that this variable is properly set and unset as required. In some places
I could do this with on_actions hooks; in others, I had to copy and modify base
OWB code.

This code is just the stuff that doesn't overwrite OWB base files. That *also*
has to happen in order for this to be useful, but I wanted it to be easy to
tell what was what for now.

"Is that all?"

Further, I've wrapped this code up using a small suite of scripted triggers and
effects. Using these, you should be able to avoid most of the boilerplate
needed for doing the following:
* Joining one of the major named factions
* Finding the leader of one of the major named factions
* Determine if a major faction exists without knowing its leader in advance
* Adding/removing AI alliance strategies with other faction members
* Adding/removing opinion modifiers with other faction members
* Properly handling strategies and opinions for puppets of faction members

"Examples?"

For example, to have New Reno join the NCR with all of the correct AI
strategies and opinions set - along with Klamath's, if it is a puppet:

NEW = { join_the_ncr = yes }

Only run code if the Nevada Pact exists - regardless of who founded it:
if = {
  limit = {
    nevada_pact_exists = yes
  }
}

"OK, so, what do you want?"

I hate having to modify base OWB game code since I'll need to keep updating my
copies of it if I use these faction scripts in my own mod. If I could convince
any official dev to consider using triggers and effects that do a good job of
tracking faction membership - it doesn't even have to be this code! - it'll
save me a lot of work.

I also argue it'll make OWB's code more consistent internally, as right now
there are lots of places where factions are joined without the appropriate AI
strategies or without the correct opinion modifiers. Using these 'functions' to
interact with factions should make those bugs much harder to make. It'll also
correct the subtle bugs that can arise from shifting faction leadership.

"What the hell are these scripts and templates?"

As you might notice, the scripting for tracking factions is pretty repetitive.
I'm thinking of ways to compress things further, but PDXScript doesn't give me
a lot of tools to do that.

Therefore, I wrote a script and some template files. You can use the script to
generate the required files for a new faction, or to regenerate fresh files for
an existing one.

Long term, I'd like to set up some configurability so you can generate these
files with custom additions per-faction - like faction specific opinion and AI
modifiers besides the simple alliance one - but I wanted to get something out
there.

Also, the script is something I wrote in 20 minutes and I need to clean it up a
lot too.
