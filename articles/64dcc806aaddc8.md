---
title: "Introducing my own plugins for GitBucket"
emoji: "🔖"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ['GitBucket']
published: true
---
## What is GitBucket?

[GitBucket](https://gitbucket.github.io/) is a web service that allows you to share [Git](https://git-scm.com/) repositories over a network, similar to [GitHub](https://github.com/).

- It is distributed under the Apache-2.0 license.
- It's developed in [Scala](https://www.scala-lang.org/) and can be run in any Java 17 environment.
- For trial use, you can use the built-in [H2](https://h2database.com/) database engine.
   - For production use, we recommend using a relational database server such as [PostgreSQL](https://www.postgresql.org/) or [MariaDB](https://mariadb.org/).
- It's super easy to self-host.

# drawio plugin (forked version)

In addition to the plugins introduced this time, I also made the [drawio plugin](https://github.com/onukura/gitbucket-drawio-plugin) work.

In [the forked version](https://github.com/yasumichi/gitbucket-drawio-plugin), we made self-hosted draw.io available.

# GitBucket Markdown Enhanced Plugin

[GitBucket Markdown Enhanced Plugin](https://github.com/yasumichi/gitbucket-markdown-enhanced) provides rich Markdown conversion features similar to [Markdown Preview Enhanced](https://shd101wyy.github.io/markdown-preview-enhanced/#/), a plugin for [Visual Studio Code](https://code.visualstudio.com/).

Although only a portion of the functionality is currently reproduced, it is possible to embed diagrams such as PlantUML and mermaid.

Currently, it is only available in repository viewer, but from GitBucket 4.45.0, it will be available in issues, wiki, etc.

- [Add support for an alternative renderer to commit comments, wikis, and issues. by yasumichi · Pull Request #3882 · gitbucket/gitbucket](https://github.com/gitbucket/gitbucket/pull/3882)

# GitBucket Flexible Gantt Plugin

[GitBucket Flexible Gantt Plugin](https://github.com/yasumichi/gitbucket-flexible-gantt-plugin) is powered by [Frappe Gantt](https://github.com/frappe/gantt).

- Add the following to the issue side bar
   - Start Date
   - End Date
   - Progress
   - Dependencies
- Display the Gantt chart from the above information
   - If you have edit permissions, start date, end date and progress rate can be changed by drag and drop.
   - Clicking on a task will open the corresponding issue.
   - You can filter by milestones and labels.
