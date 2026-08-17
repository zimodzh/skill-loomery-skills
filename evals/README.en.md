# evals

<p align="center">  <samp>
    <a href="./README.md">中文</a> ·
    <strong>English</strong>
  </samp>
</p>

Stores quality evaluation cases for a Skill.

Each case should include:

- `prompt`: A realistic user request.
- `expected_output`: Observable success criteria.
- `files`: Optional input files.
- `assertions`: Verifiable checks.

`evals.json` describes test cases; it does not run them. Keep run outputs,
timing, grades, and human feedback in a separate evaluation workspace rather
than this directory.
