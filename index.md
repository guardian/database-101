# Database 101

An introductory database course, focusing on Postgres.

## Week 1

Slide are
[here](https://docs.google.com/presentation/d/e/2PACX-1vS523DmMz-8ykd60BseWmh5gv2Zhhfzz_l6bJhJ3ql4dcpb4vGOwLDvTMmV8yAsd4R_3JX_itYIK5Ip/pub?start=false&loop=false&delayms=3000#slide=id.g293ad384ec_0_140).

* [Lesson 1 - adding data and basic queries](lessons/lesson-1.html)
* [Lesson 2 - more queries](lessons/lesson-2.html)
* [Lesson 3 - transactions and types](lessons/lesson-3.html)

Examples have been liberally borrowed from the Postgres docs and other
usable sources.

## Requirements

* being a noob
* Postgres running locally ([Postgres App](https://postgresapp.com/)
  is recommended for Macs)

Once Postgres is running, you can use `psql` at the command line to
gain access.

## Getting help and documentation

`psql` help is available using:

    \h // for SQL commands, e.g. \h SELECT
    \? // for Postgres management help, e.g. \d+ products

[Postgres documentation](https://www.postgresql.org/docs/current/static/index.html)
is great; you should use it liberally during the course.

Instructions for understanding the command synopses are
[here](https://www.postgresql.org/docs/current/static/notation.html)
so read that too.

## Contributing

Simply fork and raise a PR. The site runs on
[Github Pages](https://help.github.com/categories/github-pages-basics/).
