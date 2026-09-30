# WrightKit agent skills

Optional [Agent Skills](https://agentskills.io) for Overwatch Workshop projects. Each works on its own.

| Skill | What it does |
| --- | --- |
| [`wright`](skills/wright/SKILL.md) | Helps a coding agent decide when and how to use [Wright](https://github.com/wrightkit/wright) in Workshop and OverPy projects. Wright owns the executable tools and semantic results, and works without this guide. [Install Wright](https://github.com/wrightkit/wright) separately. |
| [`overpy`](skills/overpy/SKILL.md) | Teaches a coding agent to write OverPy and use the upstream `overpy` compiler: syntax, events, compiler errors and fixes. Every example compiles with `overpy@9.7.10`. Install the compiler with `npm install -g overpy`. |

## Install

```sh
npx skills add wrightkit/skills
```

The [Skills CLI](https://github.com/vercel-labs/skills) asks which agents and scope to use. Common variants:

```sh
# list what is available without installing
npx skills add wrightkit/skills --list

# install for one agent, or globally
npx skills add wrightkit/skills -a claude-code
npx skills add wrightkit/skills -g

# one skill only
npx skills add wrightkit/skills --skill overpy

# update later
npx skills update
```

Node is needed only to install; the skill has no runtime dependency.

### Manual install

Copy a skill directory (`SKILL.md` and `references/`) into your agent's skills directory. For agents that use `.agents/skills`:

```sh
git clone https://github.com/wrightkit/skills.git
mkdir -p <your-project>/.agents/skills
cp -R skills/skills/wright skills/skills/overpy <your-project>/.agents/skills/
```

To update, replace that directory with the current one from this repository.

A first-party `wright agent install` is tracked in [Wright #415](https://github.com/wrightkit/wright/issues/415) and will be preferred once released.

## License

[AGPL-3.0](LICENSE).
