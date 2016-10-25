Hibrow-LoDash
=============

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

map
reduce
filter

* filter github events and grab all the commits
* how many commits did everyone on your team make?
* cat your-githubz.json | ruby -r json  -e 'puts JSON.parse($stdin.read).select { |e| e["type"] == "PushEvent" }.flat_map { |e| e["payload"]["commits"] }.map { |e| e["message"] }.to_json' |  jq .
* `curl 'https://api.github.com/users/JoshCheek/events/public?page=7'  | ruby -r json  -e 'puts JSON.parse($stdin.read).select { |e| e["type"] == "PushEvent" }.flat_map { |e| e["payload"]["commits"] }.map { |e| e["message"] }.to_json' | jq .`


https://api.github.com/repos/turingschool/front-end-curriculum
https://api.github.com/repos/turingschool/front-end-curriculum/commits

Notes
* Keys are unique
