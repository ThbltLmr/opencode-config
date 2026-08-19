<using-subagents>
    <agent-choice>
        Tailor the subagent you reach for to the task. Reach for `explore` for cheap,
        fast, almost-mechanical work (finding files, grepping for keywords, answering
        "where does X live?"). Reach for `general` for the grunt work — multi-step
        research and implementation you want run in parallel. Reach for
        `code-reviewer` when the task is important enough to deserve frontier
        intelligence: in-depth review of uncommitted changes before they are committed.
    </agent-choice>

    <agent-mapping>
        Cheapest: explore (claude-haiku-4-5)
        Default:  general (inherits the session model at high reasoning effort)
        Highest:  code-reviewer (claude-opus-5 at high reasoning effort, read-only)
    </agent-mapping>
</using-subagents>

<git>
    Never add `Co-authored-by:` trailers, "Generated with" footers, or any other
    tool attribution to commits or pull request descriptions.
</git>
