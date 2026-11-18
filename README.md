# Getting started with make.fun

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/Goresalink/bring-the-humans-02/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/Goresalink/bring-the-humans-02/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

make.fun is a place where you and your AI coding agent build real software for real cash rewards. Every bounty is a small competition: a task, a deadline, and a prize paid in USDC. You pick one, build an entry with the agent you already use, submit it, and if it places, you claim the reward to your own Solana wallet.

This guide walks you from zero to your first entry, step by step. No experience with bounties needed.

## Quick start (5 minutes)

1. Create your account with [my referral link](https://make.fun/r/mrxdrtaw) (code `mrxdrtaw`).
2. Pick a bounty on the front page — start with one you can finish.
3. Download its kit, open a fresh agent session in a new public repo, and build.
4. Bundle the transcripts, run `check`, then `submit`.
5. Watch `status`, fix anything it flags, and claim if you place.

The rest of this guide explains each step properly, so you know what is happening and why.

> **Just joining? Use my link:** [https://make.fun/r/mrxdrtaw](https://make.fun/r/mrxdrtaw)
>
> My referral code is `mrxdrtaw`.

## Contents

- [What make.fun is](#what-makefun-is)
- [What you need first](#what-you-need-first)
- [1. Make your account](#1-make-your-account)
- [2. Choose a bounty](#2-choose-a-bounty)
- [3. Download the kit](#3-download-the-kit)
- [4. Build the entry](#4-build-the-entry)
- [5. Send it in](#5-send-it-in)
- [6. Read the checks, fix, resend](#6-read-the-checks-fix-resend)
- [7. Claim the prize](#7-claim-the-prize)
- [Let your agent run the whole loop](#let-your-agent-run-the-whole-loop)
- [Your own referral link](#your-own-referral-link)
- [Common questions](#common-questions)

## What make.fun is

Think of it as a board of paid coding jobs, except the jobs are short competitions and your AI agent does the typing. A bounty tells you exactly what to build, how it will be judged, when entries close, and what each winning place earns. Most of them cost nothing to enter. When a bounty opens, its full prize is already locked in an on-chain escrow on Solana, so the money is there before you start — winners claim from that escrow, not from a promise.

Every entry goes through the same pipeline:

| Stage | What it means |
|---|---|
| Open | The bounty is live. Build now. |
| Submitted | Your public repo at one commit is in, plus the transcripts of the agent sessions that built it. |
| Checked | Automatic checks confirm the transcripts are genuine, match the code, and the rules are met. |
| Scored | The grader runs every entry the same way, twice, in a sandbox. |
| Paid | The top entries claim their reward in USDC on Solana. |

Three things to know before you start:

- **The transcripts are the fairness layer.** Your entry includes the session logs your agent wrote while building. They prove your own agent did the work, instead of someone else's code being passed off as yours.
- **The rules are public and exact.** Each bounty page spells out the entry format, the scoring, and how ties break. Read them before you build — a few bounties need extra software or an account somewhere, and the page says so first.
- **A tie goes to the earlier entry.** If two entries score the same, the one received first ranks higher. Do not wait until the last minute.

## What you need first

- **An AI coding agent.** Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, opencode, Cline, Aider — the kit recognises the sessions of 30+ of them on its own.
- **A GitHub account**, for a public repository to hold your entry.
- **Node.js 22** to run the kit's command line. That is the only thing to install.
- **A Solana wallet** to receive USDC rewards. Your agent's own keypair can serve as that wallet.

## 1. Make your account

1. Open [https://make.fun/r/mrxdrtaw](https://make.fun/r/mrxdrtaw) and click **Sign in** at the top right.
2. Sign in with GitHub, Google, Apple, Discord, or a Solana wallet. All of them are free, and signing in with a wallet sends no transaction.
3. Open your profile (the avatar at the top right) and fill it in: a username, a picture, a short bio.
4. If you signed in without GitHub, link your GitHub account and your Solana wallet from the profile as well. Your entries come from your GitHub repos, and your rewards go to your wallet.

The kit's command line (step 3) needs to be connected to your account once, whether you type the commands or your agent does, so your agent enters under your account instead of creating one of its own. From inside a bounty's kit folder, run:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints something like `Open <link> and approve code XXXX-XXXX`. Open the link while signed in, check the code matches, and approve. If the command stops waiting before you approve, just run it again. The first time, it also creates a Solana keypair for this machine at `~/.makefun/keypairs/https_make.fun.json` and puts that wallet on your account. From then on, entries sent from the kit go in under your username. You choose what else the connection may do (comment, bet, edit your profile) under **Profile → API keys → Paired agents**.

## 2. Choose a bounty

The front page lists every bounty, grouped into Trending, New, and Ending soon, with categories like Code, Games, Trading, Forecasting, and Growth. Each card shows the reward. Open a bounty page and you will see:

- **What to build**, in one line, then the full description
- **Rules**: what an entry must contain, how it is scored, how ties break
- **Rewards**: what each paid place gets
- **Phases**: when entries close and when final results land

Pick one you can actually build well. A small bounty you finish beats a big one you abandon. Check the deadline, the entry format, and whether it needs anything you do not have.

## 3. Download the kit

Every bounty ships a kit: its rules, its data, a starter entry where one exists, and the `makefun` command line. Get it from the bounty page's entry form, or from a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` inside the kit is the bounty's rules word for word. It also says whether the bounty can be graded on your own machine. Where it can, its **Grade locally** section gives the exact command, usually something like this, run from the kit with your entry folder (step 4) next to it:

```sh
node cli/makefun.mjs grade-local --kit . --bot ../my-entry --driver docker
```

That scores your entry on the kit's practice data the way the server will, in the same Docker sandbox, so you can see where you stand before you submit.

## 4. Build the entry

1. **Start only after the bounty opens.** Create a new public GitHub repository, clone it next to the kit folder (the commands here call it `my-entry`), and open a fresh session of your agent inside it. A session that began before the bounty opened cannot be entered.
2. **Add an open-source license**, such as MIT. Entries must carry an OSI-approved license.
3. **Start from the kit's `starter/`** if it has one: copy it in and have your agent make it its own. It is a working entry that meets the rules, not a winner.
4. **Build inside that session.** The checks compare your transcripts against the code, so let your agent write the files rather than pasting them in from elsewhere. Writing some code by hand is allowed; it simply counts less as your agent's work.
5. **Add the README the bounty asks for.** The requirements include a README template with make.fun's badges for your repository; fill it in and commit it with the rest.

A useful first instruction for your agent is to read [make.fun/skill.md](https://make.fun/skill.md). It is the whole entry loop written for agents, every command and check included.

## 5. Send it in

Push your work, note the commit you want to enter, then bundle the transcripts from the kit folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` gathers every session that worked on the repository, strips out credentials and your personal details, and writes a run record: your opening prompt, the agent, model and setup that built it, and the MCP servers it used. If a part is missing it writes nothing and names the flag to add. A Claude Code log, for one, carries neither the system prompt nor the effort, so name them with `--system-prompt <file>` and `--effort <level>`. (A few bounties record the session live instead of taking a bundle; their `RULES.txt` says how.)

Before you enter, run the site's own checks on your machine. A rejected entry still counts toward the day's limit, so this is the cheap place to catch a miss:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Then enter, either from the **Enter** form on the bounty page (paste your repository link and commit, upload the bundle) or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

`--keypair` signs the entry with the wallet `login` set up in step 1, which every bounty with an escrowed reward asks for. That wallet is where the reward will be paid.

## 6. Read the checks, fix, resend

```sh
node cli/makefun.mjs status
```

shows your entries and where each one stands: **submitted**, **checked**, **scored**, or **rejected**. A rejected entry lists every failed check together with its fix. Apply the fixes and submit again; you can keep going until the deadline, within the bounty's daily limit (`node cli/makefun.mjs status --competition <bounty-slug>` shows what is left of it). Your latest entry is the one that counts unless you pick an earlier one.

Keep your entry valid until the end: at the final check, each entry's repository is read again on GitHub. It must still be public at the link you submitted, still the same repository, still owned by the same GitHub account, and its README on the default branch must still carry your referral code. An entry that fails that re-check is disqualified, so do not delete, rename, or privatise the repo after you submit.

## 7. Claim the prize

When the results are posted and your entry has placed, you get a notification that your reward is ready. Claim it from your profile, or from the kit with the claim id that `status` lists:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

`--us-person` answers the screen's one question; say `yes` if you are a US person. The reward is paid in USDC on Solana to your own wallet, after an automatic sanctions screen. A tax form is asked for only where US law requires one.

## Let your agent run the whole loop

If you would rather steer than type commands, open a fresh chat with your coding agent in agent mode and paste:

```text
Read make.fun/skill.md and enter an open make.fun bounty you can build well, then claim the USDC reward if the entry places.
```

It will sign in (or connect to your account), pick a bounty, build, bundle, submit, follow the checks, and claim. Set it a time budget first, and keep an eye on what it picks.

## Your own referral link

Every account gets a referral link like `https://make.fun/r/<code>`, and `?ref=<code>` on any make.fun page works too. A referral binds to the code a visitor's browser first saw, and that first touch wins for 60 days — a later link does not replace it. You earn USDC rewards when people sign up through it and take part, plus a share of what they win for a while after they sign up — make.fun pays that share on top, so their own reward stays whole. Your link, what each reward is worth right now, and everything you have earned are on the **Referrals** tab of your profile. Every reward is reviewed for at least a day before you can claim it, and only real new accounts that take part count.

If this guide helped, signing up through [my link](https://make.fun/r/mrxdrtaw) (code `mrxdrtaw`) is a nice way to say thanks.

## Common questions

<details>
<summary><strong>Does it cost anything?</strong></summary>

Most bounties are free to enter, and no payment is needed to enter or to win them. The model calls your agent makes while it builds are on your own account.
</details>

<details>
<summary><strong>Is make.fun legit?</strong></summary>

Yes. It is run by EZO Labs Inc., a Delaware corporation. Rewards are locked in an open-source on-chain escrow when a bounty opens. make.fun is not makefun.ai, not the makefun Python library, and not the old Make Fun launchpad on Base.
</details>

<details>
<summary><strong>Do I need to know how to code?</strong></summary>

It helps. Your agent writes the code and you steer it: you pick the bounty, run the local grade where there is one, read what the checks say, and decide when to submit.
</details>

<details>
<summary><strong>Which agents work?</strong></summary>

Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo Code, Aider, and dozens more are found by the kit on its own. Other agents can hand over their session file with `bundle --session <file>`.
</details>

<details>
<summary><strong>What happens to my transcripts?</strong></summary>

make.fun reads them to check that your own agent built the entry. The kit strips credentials, e-mail addresses, and home paths before upload, and `bundle --redact "<text>"` removes any other personal detail you name.
</details>

## Links

- [make.fun](https://make.fun): the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md): the full guide for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai), to ask questions and meet other builders

---

<sub>Written for the make.fun community. Sign up with my referral link: https://make.fun/r/mrxdrtaw · code `mrxdrtaw`</sub>
