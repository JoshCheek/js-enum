Hibrow-LoDash
=============

LoDash gives you some nice "enumerable" helpers (some people would call these "iterable").
These methods help you work with collections like arrays and objects.
But it's important to understand what abstractions actually do,
so lets figure out how to do those things without LoDash.


Setup
-----

Install `jq`

```sh
$ brew install jq
```

[These](https://api.github.com/users/stevekinney/events/public)
are Steve Kinney's public github events, try them out, and then try finding your own.

```sh
$ curl 'https://api.github.com/users/stevekinney/events/public'
$ curl 'https://api.github.com/users/stevekinney/events/public' | jq .
```

Quick aside: [These](https://github.com/stevekinney.keys) are Steve Kinney's public keys,
see how you can see them? That's why they're public, everyone has your public keys,
but only you have your private keys.

```sh
$ cat ~/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDdrL6VqkfrR222anFfHeibV9giF6JinYadCWZAgKH85J4SdJ+YopVbw4bB1MSKuu0+GomN7xyEcG13Fd9a9nlIkPkxoLFJC7tDA6gH6aRmoNX+pRwgX5nu2EA09PCgRQfnX+AXivYi5eDU3s0Sp8qLi/lTEouN/sJKDC1OOHAXOj46HxBk5NyoDBVINSpvGBjvhVJURRd0YOVZdi1Oe8A9fuTLWB8aT/xFucP62EN4YFq5wzSrj//xrVo3dJXCMssNaPBL2p2I32dlXMmGWdtlFHnLQsyEZrSg7jCPOBMEJ9aAHigl/cj77sdc7vtBFz3clb6x5c6zhmYGZgESfzr9 josh.cheek@gmail.com

$ curl https://github.com/JoshCheek.keys | head -1
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDdrL6VqkfrR222anFfHeibV9giF6JinYadCWZAgKH85J4SdJ+YopVbw4bB1MSKuu0+GomN7xyEcG13Fd9a9nlIkPkxoLFJC7tDA6gH6aRmoNX+pRwgX5nu2EA09PCgRQfnX+AXivYi5eDU3s0Sp8qLi/lTEouN/sJKDC1OOHAXOj46HxBk5NyoDBVINSpvGBjvhVJURRd0YOVZdi1Oe8A9fuTLWB8aT/xFucP62EN4YFq5wzSrj//xrVo3dJXCMssNaPBL2p2I32dlXMmGWdtlFHnLQsyEZrSg7jCPOBMEJ9aAHigl/cj77sdc7vtBFz3clb6x5c6zhmYGZgESfzr9
```

```sh
$ curl 'https://api.datamuse.com/words?rel_rhy=turing'                             > words-that-rhyme-with-turing.json
$ curl 'http://api.zippopotam.us/us/80202'                                         > denver-info.json
$ curl 'http://maps.googleapis.com/maps/api/geocode/json?latlng=39.7491,-104.9946' > denverish-locations.json
$ curl 'https://api.github.com/repos/turingschool/front-end-curriculum/commits'    > curriculum-commits.json
```

Some vague semblence of a plan
------------------------------

* 30 min lets play with these functions
  `node -p 'Reflect.ownKeys(Array.prototype)'`
* 90 min lets try using them on API data!

Examples (30 min)
-----------------

These are the potentially interesting methods:

```javascript
> Reflect.ownKeys(Array.prototype)
[ 'length', 'constructor', 'toString', 'toLocaleString', 'join', 'pop', 'push', 'reverse', 'shift', 'unshift',
  'slice', 'splice', 'sort', 'filter', 'forEach', 'some', 'every', 'map', 'indexOf', 'lastIndexOf', 'reduce',
  'reduceRight', 'copyWithin', 'find', 'findIndex', 'fill', 'includes', 'entries', 'keys', 'concat',
  Symbol(Symbol.unscopables), Symbol(Symbol.iterator) ]
```

* Predict the result of each of these
* Try them to see if you're right!
* Explain them in one sentence to the human adjacent to you
* Say in one sentence when you would choose to use them

```javascript
["a", "b", "c"].length     // what will this be?

["a", "b", "c"].join("-")  // what will this be?

var ary = ["a", "b", "c"]
ary.pop()                  // what will this be?
ary                        // what will this be?

ary = ["a", "b", "c"]
ary.push("d")
ary                        // what will this be?

ary = ["a", "b", "c"]
ary.concat("d")            // what will this be?
ary                        // what will this be?

ary = ["a", "b", "c", "d", "e"]
ary.slice(0)               // what will this be?
ary.slice(1)               // what will this be?
ary.slice(2)               // what will this be?
ary.slice(3)               // what will this be?
ary.slice(1,2)             // what will this be?
ary.slice(1,3)             // what will this be?
ary.slice(2,4)             // what will this be?

ary = ["a", "b", "c", "d", "e"]
ary.map((char) => char.toUpperCase()) // what will this be?
ary.map((char) => char + char)        // what will this be?

var chars = ""
["a", "b", "c", "d", "e"].forEach((char) => chars = chars + char)
chars // what will this be?

chars = ""
["a", "b", "c", "d", "e"].forEach((char) => chars = char + chars)
chars // what will this be?

"abc def ghi".split(" ")  // what will this be?


ary = ["a", "b", "c"]
for(var i of ary) console.log(i)      // what will be printed?
for(var i of ary) console.log(ary[i]) // what will be printed?

var name
var obj = {"a": 1, "b": 2, "c": 3}
for(name in obj) console.log(name)      // what will be printed?
for(name in obj) console.log(obj[name]) // what will be printed?

ary = ["a", "b", "c"]
for(name in obj) console.log(name)      // what will be printed?
for(name in obj) console.log(obj[name]) // what will be printed?


```

How to play with API data
-------------------------

```sh
$ curl 'http://api.zippopotam.us/us/80202'
{"post code": "80202", "country": "United States", "country abbreviation": "US", "places": [{"place name": "Denver", "longitude": "-104.9946", "state": "Colorado", "state abbreviation": "CO", "latitude": "39.7491"}]}⏎                                       09:33 AM   ~/code/jsl/hibrow-lodash   master


$ curl 'http://api.zippopotam.us/us/80202' | jq .
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   216  100   216    0     0    770      0 --:--:-- --:--:-- --:--:--   768
{
  "post code": "80202",
  "country": "United States",
  "country abbreviation": "US",
  "places": [
    {
      "place name": "Denver",
      "longitude": "-104.9946",
      "state": "Colorado",
      "state abbreviation": "CO",
      "latitude": "39.7491"
    }
  ]
}


$ curl 'http://api.zippopotam.us/us/80202' > denver-info.json
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   216  100   216    0     0   2688      0 --:--:-- --:--:-- --:--:--  2700


$ cat denver-info.json
{"post code": "80202", "country": "United States", "country abbreviation": "US", "places": [{"place name": "Denver", "longitude": "-104.9946", "state": "Colorado", "state abbreviation": "CO", "latitude": "39.7491"}]}


$ node -p 'require("./denver-info.json")'
{ 'post code': '80202',
  country: 'United States',
  'country abbreviation': 'US',
  places:
   [ { 'place name': 'Denver',
       longitude: '-104.9946',
       state: 'Colorado',
       'state abbreviation': 'CO',
       latitude: '39.7491' } ] }


$ node -p 'require("./denver-info.json").places[0].latitude'
39.7491
```


Try some shit out
-----------------

* What are the names of the runtime dependencies of Ruby on Rails?

  ```sh
  $ curl https://rubygems.org/api/v1/gems/rails.json
  $ curl https://rubygems.org/api/v1/gems/rails.json | jq .
  $ curl https://rubygems.org/api/v1/gems/rails.json > rails.json
  ```
* `https://api.github.com/users/{YOURNAME}/events/public`
  * filter github events and grab all the commits
  * Find the set of events you've done, this is stored on the type (don't allow duplicates)
  * Get the set of events you've done and their repo name (output should match this)

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
