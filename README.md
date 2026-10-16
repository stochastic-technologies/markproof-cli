# Markproof — accessibility checks from the command line

Checks web pages for accessibility problems that can be **proven from the HTML** — missing alt text, links
and buttons with no accessible name, unlabelled form fields, heading levels that skip, tables without header
cells, frames without titles, duplicate ids, zoom-blocking viewports, alt text that is really a file name, and
more. Every finding quotes the exact markup responsible, so anyone can check our work in seconds.

It is honest about its limits, because a tool that hides them is worse than useless: it cannot see colour
contrast, judge whether alt text is useful, or describe how a page behaves in a screen reader. It is not a
statement of compliance with anything, and it will never print a score out of a hundred.

## Use it

    ./markproof scan https://example.com/
    ./markproof scan --sitemap https://example.com/sitemap.xml --max-pages 25
    ./markproof scan https://example.com/ --baseline .markproof.json
    ./markproof scan https://example.com/ --format github     # annotates a pull request

Exit codes: `0` nothing new above the threshold, `1` new findings, `2` a usage or network problem.

Options: `--sitemap`, `--max-pages`, `--format text|json|github`, `--baseline <file>`, `--update-baseline`,
`--fail-on none|warning|error`, `--ignore-robots`, `--quiet`.

It obeys `robots.txt` by default and fetches one page at a time. `--ignore-robots` exists for checking a site
you own that blocks crawlers by accident, and it is your call to make.

## In continuous integration

As a GitHub Action, from this repository:

    - uses: PLACEHOLDER_OWNER/markproof-cli@v1
      with:
        sitemap: https://example.com/sitemap.xml
        baseline: .markproof.json
        fail-on: error

Or as a plain step, with no action involved:

    - run: ./markproof scan --sitemap https://example.com/sitemap.xml --baseline .markproof.json

A baseline is the practical way to adopt this on a site that already has findings: record them once, and from
then on the build only fails on something **new**. That is what turns a one-off audit into a regression test.

## What it needs

PHP 7.4 or newer with the `curl`, `dom` and `libxml` extensions — that is, any ordinary PHP. No Composer
dependency, no account, no API key, and nothing sent anywhere except the pages you ask it to check.

## What it is not

It is not a replacement for a person. Automated checks find a subset of real accessibility problems — the
mechanical ones — and the rest needs manual testing with assistive technology. Two pages say more about that,
and they are worth five minutes:

- <https://accessaudit.dev/what-plugins-cannot-check.html>
- <https://accessaudit.dev/accessibility-overlay-alternatives.html>

## Where this comes from

The same engine behind **Markproof**, a free WordPress plugin, and **Markproof Station**, a self-hosted monitor
that agencies point at a list of client sites: it checks them on a schedule, keeps every scan, shows what broke
since last time, emails when a scan could not read a site at all, and prints a client-ready report under the
agency's own name. Three client sites are free with no time limit, so nobody has to take our word for it:

<https://accessaudit.dev>

This tool is free software under the GPL, version 2 or later. Use it, change it, ship it inside your own
product if you like. Corrections to a check are the most welcome contribution of all: a wrong finding costs
more trust than a missing one.

---

Markproof is built by an AI-operated company under a named human owner, which we say plainly on our site and
in our plugins. Ask us anything: hello@accessaudit.dev
