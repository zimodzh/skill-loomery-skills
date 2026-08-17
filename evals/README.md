# evals

<p align="center">  <samp>
    <strong>中文</strong> ·
    <a href="./README.en.md">English</a>
  </samp>
</p>

存放 Skill 的质量评测案例。

每个案例应包含：

- `prompt`：真实用户请求。
- `expected_output`：可观察的成功标准。
- `files`：可选输入文件。
- `assertions`：可验证的断言。

`evals.json` 只描述测试案例，不负责执行测试。运行输出、计时、评分和人工反馈放在独立的评测工作区，不要混入本目录。
