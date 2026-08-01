# GitHub Profile README — gittydia Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create a GitHub profile README for user `gittydia` with auto-updating stats powered by Python + GitHub Actions.

**Architecture:** Clone of Andrew6rant's profile README approach — a Python script (`today.py`) queries the GitHub GraphQL API for stats, then updates template SVG files. A GitHub Actions workflow runs daily to keep stats current.

**Tech Stack:** Python 3.8+, `requests`, `lxml`, `python-dateutil`, GitHub Actions, GitHub GraphQL v4 API

## Global Constraints

- SVG dimensions: 985px x 530px
- Font: Consolas, 16px (with fallback @font-face)
- SVG element IDs for dynamic stats: `age_data`, `repo_data`, `contrib_data`, `star_data`, `commit_data`, `follower_data`, `loc_data`, `loc_add`, `loc_del` (plus `_dots` variants for padding)
- GitHub token permissions: read:Followers, read:Starring (account), read:Commit statuses, read:Contents, read:Metadata (repository)
- Repository name: `gittydia/gittydia`

---

### Task 1: Create `cache/requirements.txt`

**Files:**
- Create: `cache/requirements.txt`

**Interfaces:**
- Consumes: nothing
- Produces: dependency file consumed by Task 3 (today.py) and Task 4 (workflow)

- [ ] **Step 1: Create requirements.txt**

```txt
python-dateutil
requests
lxml
```

- [ ] **Step 2: Commit**

```bash
git add cache/requirements.txt
git commit -m "chore: add python dependencies"
```

---

### Task 2: Create template SVGs (`light_mode.svg` + `dark_mode.svg`)

**Files:**
- Create: `light_mode.svg`
- Create: `dark_mode.svg`

**Interfaces:**
- Consumes: nothing
- Produces: SVG template files with element IDs that Task 3 (today.py) modifies

- [ ] **Step 1: Create `light_mode.svg`**

The light mode SVG. Background `#f6f8fa`, text `#24292f`. Keys `#953800`, values `#0a3069`, additions `#1a7f37`, deletions `#cf222e`, dim text `#c2cfde`.

Left side (ASCII art, x=15, y starts at 30, increments by 20):

```svg
<?xml version='1.0' encoding='UTF-8'?>
<svg xmlns="http://www.w3.org/2000/svg" font-family="ConsolasFallback,Consolas,monospace" width="985px" height="530px" font-size="16px">
<style>
@font-face {
src: local('Consolas'), local('Consolas Bold');
font-family: 'ConsolasFallback';
font-display: swap;
-webkit-size-adjust: 109%;
size-adjust: 109%;
}
.key {fill: #953800;}
.value {fill: #0a3069;}
.addColor {fill: #1a7f37;}
.delColor {fill: #cf222e;}
.cc {fill: #c2cfde;}
text, tspan {white-space: pre;}
</style>
<rect width="985px" height="530px" fill="#f6f8fa" rx="15"/>
<text x="15" y="30" fill="#24292f" class="ascii">
<tspan x="15" y="30">   .----------------------------------------.</tspan>
<tspan x="15" y="50">  /|                                          |</tspan>
<tspan x="15" y="70"> / |   >_ whoami                              |</tspan>
<tspan x="15" y="90">/  |   >_ gittydia                            |</tspan>
<tspan x="15" y="110">|  |                                          |</tspan>
<tspan x="15" y="130">|  |   >_ status: code & create                |</tspan>
<tspan x="15" y="150">|  |   >_ mission: build & ship                |</tspan>
<tspan x="15" y="170">|  |                                          |</tspan>
<tspan x="15" y="190">|  |   >_ git log --oneline                    |</tspan>
<tspan x="15" y="210">|  |   >_ learn JS  ✓                         |</tspan>
<tspan x="15" y="230">|  |   >_ learn TS  ✓                          |</tspan>
<tspan x="15" y="250">|  |   >_ learn Python  ✓                      |</tspan>
<tspan x="15" y="270">|  |   >_ learn Rust  ...                      |</tspan>
<tspan x="15" y="290">|  |                                          |</tspan>
<tspan x="15" y="310">|  |   [code] [create] [learn]                 |</tspan>
<tspan x="15" y="330">|  |   [build] [ship] [grow]                   |</tspan>
<tspan x="15" y="350">|  |                                          |</tspan>
<tspan x="15" y="370">|  |__________________________________________|</tspan>
<tspan x="15" y="390">|  /</tspan>
<tspan x="15" y="410">| /</tspan>
<tspan x="15" y="430">|/</tspan>
<tspan x="15" y="450">'</tspan>
<tspan x="15" y="470">                                       </tspan>
<tspan x="15" y="490">                                       </tspan>
<tspan x="15" y="510">                                       </tspan>
</text>
<text x="390" y="30" fill="#24292f">
<tspan x="390" y="30">gittydia@grant</tspan> -———————————————————————————————————————————-—-
<tspan x="390" y="50" class="cc">. </tspan><tspan class="key">OS</tspan>:<tspan class="cc" id="os_data_dots"> ........................ </tspan><tspan class="value">Windows + Linux Mint + Android</tspan>
<tspan x="390" y="70" class="cc">. </tspan><tspan class="key">Uptime</tspan>:<tspan class="cc" id="age_data_dots"> ...................... </tspan><tspan class="value" id="age_data">21 years, 1 month, 27 days</tspan>
<tspan x="390" y="90" class="cc">. </tspan><tspan class="key">Host</tspan>:<tspan class="cc" id="host_data_dots"> ............................. </tspan><tspan class="value">Rizal Technological University</tspan>
<tspan x="390" y="110" class="cc">. </tspan><tspan class="key">Kernel</tspan>:<tspan class="cc" id="kernel_data_dots"> ...... </tspan><tspan class="value">student + learner + software engineer</tspan>
<tspan x="390" y="130" class="cc">. </tspan><tspan class="key">IDE</tspan>:<tspan class="cc" id="ide_data_dots"> ........................ </tspan><tspan class="value">VSCode + Antigravity</tspan>
<tspan x="390" y="150" class="cc">. </tspan>
<tspan x="390" y="170" class="cc">. </tspan><tspan class="key">Languages</tspan>.<tspan class="key">Programming</tspan>:<tspan class="cc" id="lang_prog_dots"> ..... </tspan><tspan class="value">JavaScript, TypeScript, Python</tspan>
<tspan x="390" y="190" class="cc">. </tspan><tspan class="key">Languages</tspan>.<tspan class="key">Frameworks</tspan>:<tspan class="cc" id="lang_fw_dots"> ....... </tspan><tspan class="value">React, Next.js, Tailwind CSS</tspan>
<tspan x="390" y="210" class="cc">. </tspan><tspan class="key">Languages</tspan>.<tspan class="key">Backend</tspan>:<tspan class="cc" id="lang_be_dots"> ........... </tspan><tspan class="value">Django, FastAPI, Flask</tspan>
<tspan x="390" y="230" class="cc">. </tspan><tspan class="key">Languages</tspan>.<tspan class="key">Databases</tspan>:<tspan class="cc" id="lang_db_dots"> ........ </tspan><tspan class="value">PostgreSQL, MySQL, MongoDB, SQLite</tspan>
<tspan x="390" y="250" class="cc">. </tspan><tspan class="key">Languages</tspan>.<tspan class="key">DevOps</tspan>:<tspan class="cc" id="lang_do_dots"> ............ </tspan><tspan class="value">Docker, Vercel</tspan>
<tspan x="390" y="270" class="cc">. </tspan><tspan class="key">Languages</tspan>.<tspan class="key">Real</tspan>:<tspan class="cc" id="lang_real_dots"> ................. </tspan><tspan class="value">English, Filipino</tspan>
<tspan x="390" y="290" class="cc">. </tspan>
<tspan x="390" y="310" class="cc">. </tspan><tspan class="key">Hobbies</tspan>:<tspan class="cc" id="hobby_dots"> ...................... </tspan><tspan class="value">Coding, Gaming, Reading</tspan>
<tspan x="390" y="350">- Contact</tspan> -——————————————————————————————————————————————-—-
<tspan x="390" y="370" class="cc">. </tspan><tspan class="key">Email</tspan>.<tspan class="key">Personal</tspan>:<tspan class="cc" id="email_dots"> ................. </tspan><tspan class="value">boholstdianne1@gmail.com</tspan>
<tspan x="390" y="390" class="cc">. </tspan><tspan class="key">LinkedIn</tspan>:<tspan class="cc" id="linkedin_dots"> ........................ </tspan><tspan class="value">dianne-boholst</tspan>
<tspan x="390" y="410" class="cc">. </tspan><tspan class="key">Discord</tspan>:<tspan class="cc" id="discord_dots"> ......................... </tspan><tspan class="value">ianne1</tspan>
<tspan x="390" y="450">- GitHub Stats</tspan> -—————————————————————————————————————————-—-
<tspan x="390" y="470" class="cc">. </tspan><tspan class="key">Repos</tspan>:<tspan class="cc" id="repo_data_dots"> .... </tspan><tspan class="value" id="repo_data">0</tspan> {<tspan class="key">Contributed</tspan>: <tspan class="value" id="contrib_data">0</tspan>} | <tspan class="key">Stars</tspan>:<tspan class="cc" id="star_data_dots"> ........... </tspan><tspan class="value" id="star_data">0</tspan>
<tspan x="390" y="490" class="cc">. </tspan><tspan class="key">Commmits</tspan>:<tspan class="cc" id="commit_data_dots"> ............... </tspan><tspan class="value" id="commit_data">0</tspan> | <tspan class="key">Followers</tspan>:<tspan class="cc" id="follower_data_dots"> ....... </tspan><tspan class="value" id="follower_data">0</tspan>
<tspan x="390" y="510" class="cc">. </tspan><tspan class="key">Lines of Code on GitHub</tspan>:<tspan class="cc" id="loc_data_dots">. </tspan><tspan class="value" id="loc_data">0</tspan> ( <tspan class="addColor" id="loc_add">0</tspan><tspan class="addColor">++</tspan>, <tspan id="loc_del_dots"> </tspan><tspan class="delColor" id="loc_del">0</tspan><tspan class="delColor">--</tspan> )
</text>
</svg>
```

- [ ] **Step 2: Create `dark_mode.svg`**

Same layout as light_mode.svg with dark theme colors. Background `#161b22`, text `#c9d1d9`, keys `#ffa657`, values `#a5d6ff`, additions `#3fb950`, deletions `#f85149`, dim text `#616e7f`.

```svg
<?xml version='1.0' encoding='UTF-8'?>
<svg xmlns="http://www.w3.org/2000/svg" font-family="ConsolasFallback,Consolas,monospace" width="985px" height="530px" font-size="16px">
<style>
@font-face {
src: local('Consolas'), local('Consolas Bold');
font-family: 'ConsolasFallback';
font-display: swap;
-webkit-size-adjust: 109%;
size-adjust: 109%;
}
.key {fill: #ffa657;}
.value {fill: #a5d6ff;}
.addColor {fill: #3fb950;}
.delColor {fill: #f85149;}
.cc {fill: #616e7f;}
text, tspan {white-space: pre;}
</style>
<rect width="985px" height="530px" fill="#161b22" rx="15"/>
<text x="15" y="30" fill="#c9d1d9" class="ascii">
<tspan x="15" y="30">   .----------------------------------------.</tspan>
<tspan x="15" y="50">  /|                                          |</tspan>
<tspan x="15" y="70"> / |   >_ whoami                              |</tspan>
<tspan x="15" y="90">/  |   >_ gittydia                            |</tspan>
<tspan x="15" y="110">|  |                                          |</tspan>
<tspan x="15" y="130">|  |   >_ status: code & create                |</tspan>
<tspan x="15" y="150">|  |   >_ mission: build & ship                |</tspan>
<tspan x="15" y="170">|  |                                          |</tspan>
<tspan x="15" y="190">|  |   >_ git log --oneline                    |</tspan>
<tspan x="15" y="210">|  |   >_ learn JS  ✓                         |</tspan>
<tspan x="15" y="230">|  |   >_ learn TS  ✓                          |</tspan>
<tspan x="15" y="250">|  |   >_ learn Python  ✓                      |</tspan>
<tspan x="15" y="270">|  |   >_ learn Rust  ...                      |</tspan>
<tspan x="15" y="290">|  |                                          |</tspan>
<tspan x="15" y="310">|  |   [code] [create] [learn]                 |</tspan>
<tspan x="15" y="330">|  |   [build] [ship] [grow]                   |</tspan>
<tspan x="15" y="350">|  |                                          |</tspan>
<tspan x="15" y="370">|  |__________________________________________|</tspan>
<tspan x="15" y="390">|  /</tspan>
<tspan x="15" y="410">| /</tspan>
<tspan x="15" y="430">|/</tspan>
<tspan x="15" y="450">'</tspan>
<tspan x="15" y="470">                                       </tspan>
<tspan x="15" y="490">                                       </tspan>
<tspan x="15" y="510">                                       </tspan>
</text>
<text x="390" y="30" fill="#c9d1d9">
<tspan x="390" y="30">gittydia@grant</tspan> -———————————————————————————————————————————-—-
<tspan x="390" y="50" class="cc">. </tspan><tspan class="key">OS</tspan>:<tspan class="cc" id="os_data_dots"> ........................ </tspan><tspan class="value">Windows + Linux Mint + Android</tspan>
<tspan x="390" y="70" class="cc">. </tspan><tspan class="key">Uptime</tspan>:<tspan class="cc" id="age_data_dots"> ...................... </tspan><tspan class="value" id="age_data">21 years, 1 month, 27 days</tspan>
<tspan x="390" y="90" class="cc">. </tspan><tspan class="key">Host</tspan>:<tspan class="cc" id="host_data_dots"> ............................. </tspan><tspan class="value">Rizal Technological University</tspan>
<tspan x="390" y="110" class="cc">. </tspan><tspan class="key">Kernel</tspan>:<tspan class="cc" id="kernel_data_dots"> ...... </tspan><tspan class="value">student + learner + software engineer</tspan>
<tspan x="390" y="130" class="cc">. </tspan><tspan class="key">IDE</tspan>:<tspan class="cc" id="ide_data_dots"> ........................ </tspan><tspan class="value">VSCode + Antigravity</tspan>
<tspan x="390" y="150" class="cc">. </tspan>
<tspan x="390" y="170" class="cc">. </tspan><tspan class="key">Languages</tspan>.<tspan class="key">Programming</tspan>:<tspan class="cc" id="lang_prog_dots"> ..... </tspan><tspan class="value">JavaScript, TypeScript, Python</tspan>
<tspan x="390" y="190" class="cc">. </tspan><tspan class="key">Languages</tspan>.<tspan class="key">Frameworks</tspan>:<tspan class="cc" id="lang_fw_dots"> ....... </tspan><tspan class="value">React, Next.js, Tailwind CSS</tspan>
<tspan x="390" y="210" class="cc">. </tspan><tspan class="key">Languages</tspan>.<tspan class="key">Backend</tspan>:<tspan class="cc" id="lang_be_dots"> ........... </tspan><tspan class="value">Django, FastAPI, Flask</tspan>
<tspan x="390" y="230" class="cc">. </tspan><tspan class="key">Languages</tspan>.<tspan class="key">Databases</tspan>:<tspan class="cc" id="lang_db_dots"> ........ </tspan><tspan class="value">PostgreSQL, MySQL, MongoDB, SQLite</tspan>
<tspan x="390" y="250" class="cc">. </tspan><tspan class="key">Languages</tspan>.<tspan class="key">DevOps</tspan>:<tspan class="cc" id="lang_do_dots"> ............ </tspan><tspan class="value">Docker, Vercel</tspan>
<tspan x="390" y="270" class="cc">. </tspan><tspan class="key">Languages</tspan>.<tspan class="key">Real</tspan>:<tspan class="cc" id="lang_real_dots"> ................. </tspan><tspan class="value">English, Filipino</tspan>
<tspan x="390" y="290" class="cc">. </tspan>
<tspan x="390" y="310" class="cc">. </tspan><tspan class="key">Hobbies</tspan>:<tspan class="cc" id="hobby_dots"> ...................... </tspan><tspan class="value">Coding, Gaming, Reading</tspan>
<tspan x="390" y="350">- Contact</tspan> -——————————————————————————————————————————————-—-
<tspan x="390" y="370" class="cc">. </tspan><tspan class="key">Email</tspan>.<tspan class="key">Personal</tspan>:<tspan class="cc" id="email_dots"> ................. </tspan><tspan class="value">boholstdianne1@gmail.com</tspan>
<tspan x="390" y="390" class="cc">. </tspan><tspan class="key">LinkedIn</tspan>:<tspan class="cc" id="linkedin_dots"> ........................ </tspan><tspan class="value">dianne-boholst</tspan>
<tspan x="390" y="410" class="cc">. </tspan><tspan class="key">Discord</tspan>:<tspan class="cc" id="discord_dots"> ......................... </tspan><tspan class="value">ianne1</tspan>
<tspan x="390" y="450">- GitHub Stats</tspan> -—————————————————————————————————————————-—-
<tspan x="390" y="470" class="cc">. </tspan><tspan class="key">Repos</tspan>:<tspan class="cc" id="repo_data_dots"> .... </tspan><tspan class="value" id="repo_data">0</tspan> {<tspan class="key">Contributed</tspan>: <tspan class="value" id="contrib_data">0</tspan>} | <tspan class="key">Stars</tspan>:<tspan class="cc" id="star_data_dots"> ........... </tspan><tspan class="value" id="star_data">0</tspan>
<tspan x="390" y="490" class="cc">. </tspan><tspan class="key">Commmits</tspan>:<tspan class="cc" id="commit_data_dots"> ............... </tspan><tspan class="value" id="commit_data">0</tspan> | <tspan class="key">Followers</tspan>:<tspan class="cc" id="follower_data_dots"> ....... </tspan><tspan class="value" id="follower_data">0</tspan>
<tspan x="390" y="510" class="cc">. </tspan><tspan class="key">Lines of Code on GitHub</tspan>:<tspan class="cc" id="loc_data_dots">. </tspan><tspan class="value" id="loc_data">0</tspan> ( <tspan class="addColor" id="loc_add">0</tspan><tspan class="addColor">++</tspan>, <tspan id="loc_del_dots"> </tspan><tspan class="delColor" id="loc_del">0</tspan><tspan class="delColor">--</tspan> )
</text>
</svg>
```

- [ ] **Step 3: Commit**

```bash
git add light_mode.svg dark_mode.svg
git commit -m "feat: add profile SVG templates (light + dark mode)"
```

---

### Task 3: Create `today.py`

**Files:**
- Create: `today.py`
- Test: Manual run with `python today.py`

**Interfaces:**
- Consumes: `ACCESS_TOKEN` and `USER_NAME` environment variables, SVG template files
- Produces: Updated SVG files with current GitHub stats
- Depends on: `cache/requirements.txt` (Task 1), SVG templates (Task 2)

- [ ] **Step 1: Write today.py**

```python
import datetime
from dateutil import relativedelta
import requests
import os
from lxml import etree
import time
import hashlib

HEADERS = {'authorization': 'token '+ os.environ['ACCESS_TOKEN']}
USER_NAME = os.environ['USER_NAME']
QUERY_COUNT = {'user_getter': 0, 'follower_getter': 0, 'graph_repos_stars': 0, 'recursive_loc': 0, 'graph_commits': 0, 'loc_query': 0}
BIRTHDAY = datetime.datetime(2005, 5, 18)


def daily_readme(birthday):
    diff = relativedelta.relativedelta(datetime.datetime.today(), birthday)
    return '{} {}, {} {}, {} {}{}'.format(
        diff.years, 'year' + format_plural(diff.years),
        diff.months, 'month' + format_plural(diff.months),
        diff.days, 'day' + format_plural(diff.days),
        ' 🎂' if (diff.months == 0 and diff.days == 0) else '')


def format_plural(unit):
    return 's' if unit != 1 else ''


def simple_request(func_name, query, variables):
    request = requests.post('https://api.github.com/graphql', json={'query': query, 'variables':variables}, headers=HEADERS)
    if request.status_code == 200:
        return request
    raise Exception(func_name, ' has failed with a', request.status_code, request.text, QUERY_COUNT)


def graph_commits(start_date, end_date):
    query_count('graph_commits')
    query = '''
    query($start_date: DateTime!, $end_date: DateTime!, $login: String!) {
        user(login: $login) {
            contributionsCollection(from: $start_date, to: $end_date) {
                contributionCalendar {
                    totalContributions
                }
            }
        }
    }'''
    variables = {'start_date': start_date,'end_date': end_date, 'login': USER_NAME}
    request = simple_request(graph_commits.__name__, query, variables)
    return int(request.json()['data']['user']['contributionsCollection']['contributionCalendar']['totalContributions'])


def graph_repos_stars(count_type, owner_affiliation, cursor=None, add_loc=0, del_loc=0):
    query_count('graph_repos_stars')
    query = '''
    query ($owner_affiliation: [RepositoryAffiliation], $login: String!, $cursor: String) {
        user(login: $login) {
            repositories(first: 100, after: $cursor, ownerAffiliations: $owner_affiliation) {
                totalCount
                edges {
                    node {
                        ... on Repository {
                            nameWithOwner
                            stargazers {
                                totalCount
                            }
                        }
                    }
                }
                pageInfo {
                    endCursor
                    hasNextPage
                }
            }
        }
    }'''
    variables = {'owner_affiliation': owner_affiliation, 'login': USER_NAME, 'cursor': cursor}
    request = simple_request(graph_repos_stars.__name__, query, variables)
    if request.status_code == 200:
        if count_type == 'repos':
            return request.json()['data']['user']['repositories']['totalCount']
        elif count_type == 'stars':
            return stars_counter(request.json()['data']['user']['repositories']['edges'])


def recursive_loc(owner, repo_name, data, cache_comment, addition_total=0, deletion_total=0, my_commits=0, cursor=None):
    query_count('recursive_loc')
    query = '''
    query ($repo_name: String!, $owner: String!, $cursor: String) {
        repository(name: $repo_name, owner: $owner) {
            defaultBranchRef {
                target {
                    ... on Commit {
                        history(first: 100, after: $cursor) {
                            totalCount
                            edges {
                                node {
                                    ... on Commit {
                                        committedDate
                                    }
                                    author {
                                        user {
                                            id
                                        }
                                    }
                                    deletions
                                    additions
                                }
                            }
                            pageInfo {
                                endCursor
                                hasNextPage
                            }
                        }
                    }
                }
            }
        }
    }'''
    variables = {'repo_name': repo_name, 'owner': owner, 'cursor': cursor}
    request = requests.post('https://api.github.com/graphql', json={'query': query, 'variables':variables}, headers=HEADERS)
    if request.status_code == 200:
        if request.json()['data']['repository']['defaultBranchRef'] != None:
            return loc_counter_one_repo(owner, repo_name, data, cache_comment, request.json()['data']['repository']['defaultBranchRef']['target']['history'], addition_total, deletion_total, my_commits)
        else: return 0
    force_close_file(data, cache_comment)
    if request.status_code == 403:
        raise Exception('Too many requests in a short amount of time!\nYou\'ve hit the non-documented anti-abuse limit!')
    raise Exception('recursive_loc() has failed with a', request.status_code, request.text, QUERY_COUNT)


def loc_counter_one_repo(owner, repo_name, data, cache_comment, history, addition_total, deletion_total, my_commits):
    for node in history['edges']:
        if node['node']['author']['user'] == OWNER_ID:
            my_commits += 1
            addition_total += node['node']['additions']
            deletion_total += node['node']['deletions']

    if history['edges'] == [] or not history['pageInfo']['hasNextPage']:
        return addition_total, deletion_total, my_commits
    else: return recursive_loc(owner, repo_name, data, cache_comment, addition_total, deletion_total, my_commits, history['pageInfo']['endCursor'])


def loc_query(owner_affiliation, comment_size=0, force_cache=False, cursor=None, edges=[]):
    query_count('loc_query')
    query = '''
    query ($owner_affiliation: [RepositoryAffiliation], $login: String!, $cursor: String) {
        user(login: $login) {
            repositories(first: 60, after: $cursor, ownerAffiliations: $owner_affiliation) {
            edges {
                node {
                    ... on Repository {
                        nameWithOwner
                        defaultBranchRef {
                            target {
                                ... on Commit {
                                    history {
                                        totalCount
                                        }
                                    }
                                }
                            }
                        }
                    }
                }
                pageInfo {
                    endCursor
                    hasNextPage
                }
            }
        }
    }'''
    variables = {'owner_affiliation': owner_affiliation, 'login': USER_NAME, 'cursor': cursor}
    request = simple_request(loc_query.__name__, query, variables)
    if request.json()['data']['user']['repositories']['pageInfo']['hasNextPage']:
        edges += request.json()['data']['user']['repositories']['edges']
        return loc_query(owner_affiliation, comment_size, force_cache, request.json()['data']['user']['repositories']['pageInfo']['endCursor'], edges)
    else:
        return cache_builder(edges + request.json()['data']['user']['repositories']['edges'], comment_size, force_cache)


def cache_builder(edges, comment_size, force_cache, loc_add=0, loc_del=0):
    cached = True
    filename = 'cache/'+hashlib.sha256(USER_NAME.encode('utf-8')).hexdigest()+'.txt'
    try:
        with open(filename, 'r') as f:
            data = f.readlines()
    except FileNotFoundError:
        data = []
        if comment_size > 0:
            for _ in range(comment_size): data.append('This line is a comment block. Write whatever you want here.\n')
        with open(filename, 'w') as f:
            f.writelines(data)

    if len(data)-comment_size != len(edges) or force_cache:
        cached = False
        flush_cache(edges, filename, comment_size)
        with open(filename, 'r') as f:
            data = f.readlines()

    cache_comment = data[:comment_size]
    data = data[comment_size:]
    for index in range(len(edges)):
        repo_hash, commit_count, *__ = data[index].split()
        if repo_hash == hashlib.sha256(edges[index]['node']['nameWithOwner'].encode('utf-8')).hexdigest():
            try:
                if int(commit_count) != edges[index]['node']['defaultBranchRef']['target']['history']['totalCount']:
                    owner, repo_name = edges[index]['node']['nameWithOwner'].split('/')
                    loc = recursive_loc(owner, repo_name, data, cache_comment)
                    data[index] = repo_hash + ' ' + str(edges[index]['node']['defaultBranchRef']['target']['history']['totalCount']) + ' ' + str(loc[2]) + ' ' + str(loc[0]) + ' ' + str(loc[1]) + '\n'
            except TypeError:
                data[index] = repo_hash + ' 0 0 0 0\n'
    with open(filename, 'w') as f:
        f.writelines(cache_comment)
        f.writelines(data)
    for line in data:
        loc = line.split()
        loc_add += int(loc[3])
        loc_del += int(loc[4])
    return [loc_add, loc_del, loc_add - loc_del, cached]


def flush_cache(edges, filename, comment_size):
    with open(filename, 'r') as f:
        data = []
        if comment_size > 0:
            data = f.readlines()[:comment_size]
    with open(filename, 'w') as f:
        f.writelines(data)
        for node in edges:
            f.write(hashlib.sha256(node['node']['nameWithOwner'].encode('utf-8')).hexdigest() + ' 0 0 0 0\n')


def force_close_file(data, cache_comment):
    filename = 'cache/'+hashlib.sha256(USER_NAME.encode('utf-8')).hexdigest()+'.txt'
    with open(filename, 'w') as f:
        f.writelines(cache_comment)
        f.writelines(data)
    print('There was an error while writing to the cache file. The file,', filename, 'has had the partial data saved and closed.')


def stars_counter(data):
    total_stars = 0
    for node in data: total_stars += node['node']['stargazers']['totalCount']
    return total_stars


def svg_overwrite(filename, age_data, commit_data, star_data, repo_data, contrib_data, follower_data, loc_data):
    tree = etree.parse(filename)
    root = tree.getroot()
    justify_format(root, 'commit_data', commit_data, 22)
    justify_format(root, 'star_data', star_data, 14)
    justify_format(root, 'repo_data', repo_data, 6)
    justify_format(root, 'contrib_data', contrib_data)
    justify_format(root, 'follower_data', follower_data, 10)
    justify_format(root, 'loc_data', loc_data[2], 9)
    justify_format(root, 'loc_add', loc_data[0])
    justify_format(root, 'loc_del', loc_data[1], 7)
    tree.write(filename, encoding='utf-8', xml_declaration=True)


def justify_format(root, element_id, new_text, length=0):
    if isinstance(new_text, int):
        new_text = f"{'{:,}'.format(new_text)}"
    new_text = str(new_text)
    find_and_replace(root, element_id, new_text)
    just_len = max(0, length - len(new_text))
    if just_len <= 2:
        dot_map = {0: '', 1: ' ', 2: '. '}
        dot_string = dot_map[just_len]
    else:
        dot_string = ' ' + ('.' * just_len) + ' '
    find_and_replace(root, f"{element_id}_dots", dot_string)


def find_and_replace(root, element_id, new_text):
    element = root.find(f".//*[@id='{element_id}']")
    if element is not None:
        element.text = new_text


def commit_counter(comment_size):
    total_commits = 0
    filename = 'cache/'+hashlib.sha256(USER_NAME.encode('utf-8')).hexdigest()+'.txt'
    with open(filename, 'r') as f:
        data = f.readlines()
    cache_comment = data[:comment_size]
    data = data[comment_size:]
    for line in data:
        total_commits += int(line.split()[2])
    return total_commits


def user_getter(username):
    query_count('user_getter')
    query = '''
    query($login: String!){
        user(login: $login) {
            id
            createdAt
        }
    }'''
    variables = {'login': username}
    request = simple_request(user_getter.__name__, query, variables)
    return {'id': request.json()['data']['user']['id']}, request.json()['data']['user']['createdAt']


def follower_getter(username):
    query_count('follower_getter')
    query = '''
    query($login: String!){
        user(login: $login) {
            followers {
                totalCount
            }
        }
    }'''
    request = simple_request(follower_getter.__name__, query, {'login': username})
    return int(request.json()['data']['user']['followers']['totalCount'])


def query_count(funct_id):
    global QUERY_COUNT
    QUERY_COUNT[funct_id] += 1


def perf_counter(funct, *args):
    start = time.perf_counter()
    funct_return = funct(*args)
    return funct_return, time.perf_counter() - start


def formatter(query_type, difference, funct_return=False, whitespace=0):
    print('{:<23}'.format('   ' + query_type + ':'), sep='', end='')
    print('{:>12}'.format('%.4f' % difference + ' s ')) if difference > 1 else print('{:>12}'.format('%.4f' % (difference * 1000) + ' ms'))
    if whitespace:
        return f"{'{:,}'.format(funct_return): <{whitespace}}"
    return funct_return


if __name__ == '__main__':
    print('Calculation times:')
    user_data, user_time = perf_counter(user_getter, USER_NAME)
    OWNER_ID, acc_date = user_data
    formatter('account data', user_time)
    age_data, age_time = perf_counter(daily_readme, BIRTHDAY)
    formatter('age calculation', age_time)
    total_loc, loc_time = perf_counter(loc_query, ['OWNER', 'COLLABORATOR', 'ORGANIZATION_MEMBER'], 7)
    formatter('LOC (cached)', loc_time) if total_loc[-1] else formatter('LOC (no cache)', loc_time)
    commit_data, commit_time = perf_counter(commit_counter, 7)
    star_data, star_time = perf_counter(graph_repos_stars, 'stars', ['OWNER'])
    repo_data, repo_time = perf_counter(graph_repos_stars, 'repos', ['OWNER'])
    contrib_data, contrib_time = perf_counter(graph_repos_stars, 'repos', ['OWNER', 'COLLABORATOR', 'ORGANIZATION_MEMBER'])
    follower_data, follower_time = perf_counter(follower_getter, USER_NAME)

    for index in range(len(total_loc)-1): total_loc[index] = '{:,}'.format(total_loc[index])

    svg_overwrite('dark_mode.svg', age_data, commit_data, star_data, repo_data, contrib_data, follower_data, total_loc[:-1])
    svg_overwrite('light_mode.svg', age_data, commit_data, star_data, repo_data, contrib_data, follower_data, total_loc[:-1])

    print('\033[F\033[F\033[F\033[F\033[F\033[F\033[F\033[F',
        '{:<21}'.format('Total function time:'), '{:>11}'.format('%.4f' % (user_time + age_time + loc_time + commit_time + star_time + repo_time + contrib_time)),
        ' s \033[E\033[E\033[E\033[E\033[E\033[E\033[E\033[E', sep='')

    print('Total GitHub GraphQL API calls:', '{:>3}'.format(sum(QUERY_COUNT.values())))
    for funct_name, count in QUERY_COUNT.items(): print('{:<28}'.format('   ' + funct_name + ':'), '{:>6}'.format(count))
```

- [ ] **Step 2: Verify script runs**

Run: `$env:ACCESS_TOKEN="<your-token>"; $env:USER_NAME="gittydia"; python today.py`
Expected: Prints calculation times, then updates light_mode.svg and dark_mode.svg with current stats.

- [ ] **Step 3: Commit**

```bash
git add today.py
git commit -m "feat: add today.py for dynamic GitHub stats"
```

---

### Task 4: Create GitHub Actions workflow

**Files:**
- Create: `.github/workflows/build.yaml`

**Interfaces:**
- Consumes: `today.py`, `cache/requirements.txt`
- Produces: automated daily stat updates

- [ ] **Step 1: Create `.github/workflows/build.yaml`**

```yaml
name: README build
on:
  push:
    branches:
      - main
  schedule:
    - cron: "0 4 * * *"
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v2
        with:
          fetch-depth: 1
      - name: Get Python 3.8
        uses: actions/setup-python@v2
        with:
          python-version: '3.8'
      - name: Configure pip cache
        uses: actions/cache@v2
        with:
          path: ~/.cache/pip
          key: ${{ runner.os }}-pip-${{ hashFiles('**/cache/requirements.txt') }}
          restore-keys: ${{ runner.os }}-pip-
      - name: Install dependencies
        run: python -m pip install -r cache/requirements.txt
      - name: Update README file
        env:
          ACCESS_TOKEN: ${{ secrets.ACCESS_TOKEN }}
          USER_NAME: ${{ secrets.USER_NAME }}
        run: python today.py
      - name: Commit
        run: |-
          git add .
          git diff
          git config --global user.email "github-actions-bot@gittydia.github.io"
          git config --global user.name "gittydia/GitHub-Actions-Bot"
          git commit -m "Updated README" -a || echo "No changes to commit"
          git push
```

- [ ] **Step 2: Commit**

```bash
git add .github/workflows/build.yaml
git commit -m "ci: add GitHub Actions workflow for daily stats update"
```

---

### Task 5: Create README.md

**Files:**
- Create: `README.md`

**Interfaces:**
- Consumes: SVG template files
- Produces: Profile README visible on GitHub profile

- [ ] **Step 1: Create README.md**

```markdown
<a href="https://github.com/gittydia/gittydia">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/gittydia/gittydia/main/dark_mode.svg">
    <img alt="gittydia's GitHub Profile README" src="https://raw.githubusercontent.com/gittydia/gittydia/main/light_mode.svg">
  </picture>
</a>
```

- [ ] **Step 2: Commit**

```bash
git add README.md
git commit -m "feat: add profile README with dark/light mode SVGs"
```

---

### Task 6: User Setup (Manual Steps)

**Files:**
- None (GitHub UI actions)

- [ ] **Step 1: Create the `gittydia/gittydia` repository on GitHub**
  - Go to https://github.com/new
  - Repository name must be exactly `gittydia`
  - Description: "My personal repository"
  - Public
  - Do NOT initialize with README (we have our own)

- [ ] **Step 2: Create a Fine-Grained Personal Access Token**
  - Go to GitHub Settings → Developer settings → Personal access tokens → Fine-grained tokens
  - Generate new token
  - Repository access: `Only select repositories` → select `gittydia`
  - Permissions:
    - Account permissions: read:Followers, read:Starring
    - Repository permissions: read:Commit statuses, read:Contents, read:Metadata

- [ ] **Step 3: Add secrets to repository**
  - Go to `https://github.com/gittydia/gittydia/settings/secrets/actions`
  - Add `ACCESS_TOKEN` with the token value
  - Add `USER_NAME` with value `gittydia`

- [ ] **Step 4: Push code and verify**
  - Connect local repo to GitHub:
    ```bash
    git remote add origin https://github.com/gittydia/gittydia.git
    git branch -M main
    git push -u origin main
    ```
  - Wait for Actions to run (should trigger on push)
  - Check `https://github.com/gittydia/gittydia/actions` for workflow status
  - Verify profile at `https://github.com/gittydia` shows the README
