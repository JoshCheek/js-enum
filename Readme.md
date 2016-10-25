Hibrow-LoDash
=============

Setup
-----

* Install `jq`

```sh
$ brew install jq
```

* Curl output of several API endpoints into files to work with

```sh
$ curl 'https://api.github.com/users/{YOURNAME}/events/public'                     > your-githubz.json
$ curl 'https://api.datamuse.com/words?rel_rhy=turing'                             > words-that-rhyme-with-turing.json
$ curl 'http://api.zippopotam.us/us/80202'                                         > denver-info.json
$ curl 'http://maps.googleapis.com/maps/api/geocode/json?latlng=39.7491,-104.9946' > denverish-locations.json
$ curl 'https://api.github.com/repos/turingschool/front-end-curriculum/commits'    > curriculum-commits.json
```

Try some shit out
-----------------

* your-githubz.json
  * filter github events and grab all the commits
  * Find the set of events you've done (no duplicates)
  * Get the set of events you've done and their repo name

    ```sh
    $ cat your-githubz.json | jq 'map({name: .repo.name, type: .type})'
    ```
* curriculum-commits.json
  * Avatar urls of the committers to your curriculum
  * Then put them on an html page
  * The set of unique words in commit messages
  * The set of unique words (with count) in commit messages (both case sensitive and insensitive)
  * The most frequently used word in a commit message
  * Same, but omit common words (the, a, in, etc)
  * Same, but the top 2 words
  * Same, but the top 10 words
  * For each of the top 10 most frequently used words in a commit message, they should be a key in an object, with a value that shows how many times they were used
  * Same, but instead of the count, map them to a list of words that rhyme with them
  * Grand finale
    * Translate into an html file with their avatar, name, image, link to the commit
  * For each committer, their name is a key in an obj, the value is an array of the times that they committed (get it off the push event)
* denver-info.json
* denverish-locations.json
* other
  * how many commits did everyone on your team make?
  * cat your-githubz.json | ruby -r json  -e 'puts JSON.parse($stdin.read).select { |e| e["type"] == "PushEvent" }.flat_map { |e| e["payload"]["commits"] }.map { |e| e["message"] }.to_json' |  jq .
  * `curl 'https://api.github.com/users/JoshCheek/events/public?page=7'  | ruby -r json  -e 'puts JSON.parse($stdin.read).select { |e| e["type"] == "PushEvent" }.flat_map { |e| e["payload"]["commits"] }.map { |e| e["message"] }.to_json' | jq .`

Things that humans want me to make sure you know
------------------------------------------------

* Keys are unique
* Arrow functions implicitly return
* duplicated data vs reference to the same object in memory
