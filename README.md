# Which is the fastest?

[![Build Status](https://travis-ci.com/the-benchmarker/web-frameworks.svg?branch=master)](https://travis-ci.com/the-benchmarker/web-frameworks)
[![Join the chat at https://gitter.im/which_is_the_fastest/Lobby](https://badges.gitter.im/which_is_the_fastest/Lobby.svg)](https://gitter.im/which_is_the_fastest/Lobby)

This project aims to be a load benchmarking suite, no more, no less

> Measuring response times (routing times) for each framework (middleware).


<div align="center">
  :warning::warning::warning::warning::warning::warning::warning::warning:
</div>

<div align="center">Results are not <b>production-ready</b> <i>yet</i></div>

<div align="center">
  :warning::warning::warning::warning::warning::warning::warning::warning:
</div>

### Additional purposes :

+ Helping decide between languages, depending on use case
+ Learning languages, best practices, devops culture ...
+ Having fun :heart:

## Requirements

+ [Crystal](https://crystal-lang.org) as `built-in` tools are made in this language
+ [Docker](https://www.docker.com) as **frameworks** are `isolated` into _containers_
+ [wrk](https://github.com/wg/wrk) as benchmarking tool, `>= 4.1.0`
+ [postgresql](https://www.postgresql.org) to store data, `>= 10`

:information_source::information_source::information_source::information_source::information_source:

:warning: On `OSX` you need `docker-machine` to use `docker` containerization

~~~
brew install docker-machine
docker-machine create default
eval $(docker-machine env default)
~~~

:information_source::information_source::information_source::information_source::information_source:

## Usage

+ Install all dependencies

~~~sh
shards install
~~~

+ Build internal tools

~~~sh
shards build
~~~

+ Create and initialize the database

~~~sh
createdb -U postgres benchmark
psql -U postgres -d benchmark < .ci/dump.sql
~~~

Docker can be used to set up the database:

~~~sh
docker run -it --rm -d \
  -p 5432:5432 \
  -e POSTGRES_DB=benchmark \
  -e POSTGRES_HOST_AUTH_METHOD=trust \
  -v /tmp/pg-data:/var/lib/postgresql/data \
  --name pg postgres:12-alpine
~~~

Wait several seconds for the container to start, then inject the dump:

~~~sh
docker exec pg sh -c "echo \"$(cat .ci/dump.sql)\" | psql -U postgres -d benchmark"
~~~

After creating the database, export its URL:

~~~sh
export DATABASE_URL="postgresql://postgres@localhost/benchmark"
~~~

+ Make configuration

~~~sh
bin/make config
~~~

+ Build containers

> jobs are either languages (example : crystal) or frameworks (example : router.cr)

~~~sh
bin/neph [job1] [job2] [job3] ...
~~~

+ Export all results readme

~~~sh
bin/db to_readme
~~~

## Results

:information_source:  Updated on **2020-05-16** :information_source:

> Benchmarking with [wrk](https://github.com/wg/wrk)
   + Threads : 8
   + Timeout : 8
   + Duration : 15s (seconds)

:information_source: Sorted by max `req/s` on concurrency **64** :information_source:

|    | Language | Framework | 16 | 32 | 64 | 128 | 256 | 512 |
|----|----------|-----------|---:|---:|---:|----:|----:|----:|
| 1 | crystal (0.34)| [orion](https://github.com/obsidian/orion) (2.3) | 92 295 | 126 540 | 135 173 | 114 082 | 110 375 | 108 044 |
| 2 | rust (1.43)| [actix](https://actix.rs) (2.0) | 97 687 | 124 488 | 131 494 | 116 425 | 107 393 | 104 036 |
| 3 | go (1.14)| [atreugo](https://github.com/savsgio/atreugo/blob/master/docs/README.md) (11.1) | 90 654 | 118 114 | 130 361 | 112 585 | 107 473 | 104 768 |
| 4 | go (1.14)| [fiber](https://gofiber.io) (1.9) | 94 265 | 113 026 | 125 755 | 111 042 | 105 321 | 101 914 |
| 5 | go (1.14)| [router](https://pkg.go.dev/github.com/fasthttp/router) (1.1) | 94 049 | 116 430 | 120 775 | 112 379 | 104 947 | 101 772 |
| 6 | rust (1.43)| [gotham](https://gotham.rs) (0.4) | 92 357 | 115 117 | 120 586 | 108 769 | 102 288 | 100 531 |
| 7 | nim (1.2)| [jester](https://github.com/dom96/jester) (0.4) | 93 925 | 112 597 | 120 264 | 112 232 | 106 039 | 104 311 |
| 8 | go (1.14)| [goroute](https://goroute.github.io) (0.0) | 86 876 | 109 436 | 119 686 | 106 081 | 99 711 | 96 267 |
| 9 | crystal (0.34)| [athena](https://github.com/athena-framework/athena) (0.8) | 92 116 | 114 674 | 119 047 | 107 608 | 102 889 | 98 476 |
| 10 | crystal (0.34)| [amber](https://amberframework.org) (0.34) | 99 676 | 117 989 | 118 503 | 112 156 | 102 578 | 99 164 |
| 11 | crystal (0.34)| [grip](https://github.com/grip-framework/grip) (0.28) | 94 173 | 116 358 | 116 712 | 110 142 | 104 828 | 100 790 |
| 12 | kotlin (1.3)| [kooby](https://jooby.io) (2.8) | 97 238 | 115 935 | 116 152 | 111 707 | 108 886 | 105 805 |
| 13 | go (1.14)| [gorouter](https://github.com/vardius/gorouter/wiki) (4.4) | 83 700 | 110 402 | 115 103 | 108 958 | 101 787 | 99 312 |
| 14 | go (1.14)| [gorilla-mux](https://www.gorillatoolkit.org/pkg/mux) (1.7) | 83 630 | 106 461 | 113 714 | 106 153 | 101 413 | 99 650 |
| 15 | go (1.14)| [fasthttp](https://pkg.go.dev/github.com/valyala/fasthttp) (1.12) | 86 178 | 111 369 | 112 326 | 98 634 | 88 509 | 85 849 |
| 16 | crystal (0.34)| [spider-gazelle](https://spider-gazelle.net) (3.0) | 91 979 | 107 467 | 111 432 | 97 666 | 91 023 | 87 125 |
| 17 | crystal (0.34)| [onyx](https://onyxframework.org) (0.5) | 87 705 | 110 516 | 110 406 | 108 517 | 99 724 | 97 909 |
| 18 | go (1.14)| [gin](https://gin-gonic.com) (1.6) | 87 919 | 105 173 | 110 132 | 109 937 | 106 655 | 104 414 |
| 19 | scala (2.13)| [akkahttp](https://akka.io) (10.1) | 75 612 | 101 537 | 109 966 | 109 299 | 105 176 | 100 540 |
| 20 | go (1.14)| [kami](https://github.com/guregu/kami) (2.2) | 82 030 | 100 746 | 109 368 | 98 267 | 90 119 | 86 258 |
| 21 | go (1.14)| [webgo](https://github.com/bnkamalesh/webgo) (3.0) | 87 839 | 99 997 | 109 367 | 99 914 | 97 010 | 94 679 |
| 22 | nim (1.2)| [httpbeast](https://github.com/dom96/httpbeast) (0.2) | 95 476 | 115 858 | 109 064 | 97 819 | 88 848 | 85 487 |
| 23 | go (1.14)| [chi](https://github.com/go-chi/chi) (4.1) | 82 810 | 96 626 | 108 796 | 100 197 | 91 077 | 87 292 |
| 24 | kotlin (1.3)| [ktor](https://ktor.io) (1.2) | 70 013 | 100 260 | 108 370 | 104 967 | 100 623 | 96 854 |
| 25 | rust (1.43)| [iron](https://ironframework.io) (0.6) | 86 834 | 109 954 | 107 693 | 106 866 | 110 724 | 109 337 |
| 26 | clojure (1.1)| [coast](https://coastonclojure.com) (1.0) | 81 894 | 104 093 | 104 759 | 96 420 | 90 979 | 89 596 |
| 27 | go (1.14)| [beego](https://beego.me) (1.12) | 83 560 | 98 924 | 100 762 | 95 016 | 94 458 | 92 058 |
| 28 | go (1.14)| [rte](https://github.com/jwilner/rte) (0.0) | 89 853 | 113 045 | 100 133 | 93 061 | 86 242 | 82 908 |
| 29 | go (1.14)| [gf](https://goframe.org) (1.12) | 81 827 | 97 125 | 99 860 | 100 205 | 98 809 | 95 230 |
| 30 | crystal (0.34)| [lucky](https://luckyframework.org) (0.21) | 83 952 | 103 007 | 98 726 | 109 370 | 101 228 | 104 526 |
| 31 | go (1.14)| [gorouter-fasthttp](https://github.com/vardius/gorouter/wiki) (4.4) | 86 328 | 99 970 | 97 601 | 98 686 | 91 713 | 90 180 |
| 32 | go (1.14)| [violetear](https://violetear.org) (7.0) | 79 560 | 90 717 | 91 839 | 80 455 | 74 070 | 70 539 |
| 33 | go (1.14)| [air](https://github.com/aofei/air) (0.16) | 81 980 | 88 893 | 89 619 | 84 583 | 85 309 | 81 704 |
| 34 | go (1.14)| [fasthttprouter](https://pkg.go.dev/github.com/buaazp/fasthttprouter) (0.1) | 75 950 | 83 979 | 89 448 | 73 590 | 70 687 | 68 161 |
| 35 | go (1.14)| [mars](https://github.com/roblillack/mars) (1.0) | 73 073 | 83 710 | 89 222 | 81 053 | 78 899 | 74 757 |
| 36 | go (1.14)| [httprouter](https://pkg.go.dev/github.com/julienschmidt/httprouter) (1.3) | 78 602 | 86 485 | 81 567 | 76 299 | 70 289 | 68 120 |
| 37 | crystal (0.34)| [kemal](https://kemalcr.com) (0.26) | 81 390 | 96 980 | 81 513 | 67 522 | 62 499 | 59 332 |
| 38 | crystal (0.34)| [router.cr](https://github.com/tbrand/router.cr) (0.2) | 84 876 | 92 661 | 77 437 | 63 061 | 58 391 | 56 033 |
| 39 | go (1.14)| [echo](https://echo.labstack.com) (4.1) | 73 312 | 88 221 | 73 404 | 60 567 | 51 882 | 49 080 |
| 40 | scala (2.13)| [http4s](https://http4s.org) (0.21) | 52 419 | 67 593 | 70 365 | 64 332 | 62 859 | 62 242 |
| 41 | crystal (0.34)| [toro](https://github.com/soveran/toro) (0.4) | 75 836 | 88 628 | 69 707 | 57 008 | 53 210 | 48 472 |
| 42 | go (1.14)| [aero](https://github.com/aerogo/aero) (1.3) | 74 387 | 70 619 | 69 599 | 61 239 | 56 420 | 54 090 |
| 43 | clojure (1.1)| [luminus](https://luminusweb.com) (1.0) | 43 825 | 61 035 | 67 670 | 70 799 | 71 739 | 70 202 |
| 44 | rust (1.43)| [nickel](https://nickel-org.github.io) (0.11) | 67 474 | 41 435 | 63 576 | 67 079 | 65 816 | 67 527 |
| 45 | crystal (0.34)| [shivneri](https://github.com/ujjwalguptaofficial/shivneri) (0.15) | 48 460 | 49 185 | 54 157 | 55 331 | 52 034 | 50 587 |
| 46 | go (1.14)| [gramework](https://github.com/gramework/gramework) (1.7) | 33 229 | 36 894 | 45 521 | 49 407 | 48 584 | 49 011 |
| 47 | go (1.14)| [tango](https://gitea.com/lunny/tango) (0.6) | 37 616 | 37 184 | 38 738 | 41 094 | 38 289 | 38 453 |
| 48 | pony (0.35)| [jennet](https://github.com/Theodus/jennet) (0.1) | 345 | 776 | 1 549 | 2 049 | 2 026 | 974 |

## How to contribute ?

In any way you want ...

+ Request a framework addition
+ Report a bug (on any implementation)
+ Suggest an idea
+ ...

Any kind of idea is :heart:

## Contributors

- [Taichiro Suzuki](https://github.com/tbrand) - Author | Maintainer
- [OvermindDL1](https://github.com/OvermindDL1) - Maintainer
- [Marwan Rabbâa](https://github.com/waghanza) - Maintainer
