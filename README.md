# Wright agent guide

This repository distributes one optional [Agent Skill](https://agentskills.io): [`wright`](skills/wright/SKILL.md). It helps a coding agent discover and use [Wright](https://github.com/wrightkit/wright) for Overwatch Workshop projects. Wright owns the executable tools and semantic results; the guide adds no runtime dependencies or Workshop reference corpus. Wright also works without this guide.

## Install

Once Wright's first-party agent installer is released, prefer `wright agent install` from an installed Wright CLI. Check `wright agent --help` for the options supported by your version. The installer is tracked in [Wright #415](https://github.com/wrightkit/wright/issues/415); until it ships, use either method below.

For a project-local manual install, copy the single skill directory into your agent's project-local skills directory. For agents that use `.agents/skills`:

```sh
git clone https://github.com/wrightkit/skills.git
mkdir -p <your-project>/.agents/skills
cp -R skills/skills/wright <your-project>/.agents/skills/
```

Replace the destination with your agent's documented skills directory if it differs. The installed `wright` directory needs only `SKILL.md`; Wright itself must be installed separately. To update a manual installation, replace that directory with the current one from this repository.

Generic Agent Skills installation is also available where the agent supports it:

```sh
npx skills add wrightkit/skills
```

The generic installer uses Node for installation only. The guide has no npm dependency at use time.

## License

[AGPL-3.0](LICENSE).
