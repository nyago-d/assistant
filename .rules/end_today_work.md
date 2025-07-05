```yaml
trigger: [ "おつかれさま", "振り返り" ]
objective: [ "その日の振り返りを行い、課題や気付きを翌日以降の仕事に活かす。", "その日の行動や思考に関わる必要な情報を抽出し、ログに残す。" ]
steps:

    - if: not ${current_date}

      - name: "init_date"
        action: "call"
        target: "common/date_init.md"

    - name: "read_todo_memo_files"
      action: "read_files"
      path: [ "todo/${current_date}_todo.md", "memo/${current_date}_memo.md" ]
      message: "本日のTODOリスト、MEMOファイルを読み込みました。"

    - name: "review_completed_tasks"
      action: "analyze_tasks"
      source: "todo/${current_date}_todo.md"
      message: "完了済みのタスクから成果を振り返ります。"

    - name: "generate_insight_questions"
      action: "generate_questions"
      input: "${context}"
      note: "ユーザに気づきを与えるような、洞察に富んだ質問を生成します。"
      output: "insight_questions"

    - name: "ask_questions"
      action: "interactive_prompt"
      questions: "${insight_questions}"
      note: "質問は3～5程度で、まとめて出力します。番号を付けてユーザがまとめて回答しやすいようにしてください。"
      output: "user_answers"

    - name: "create_todays_retrospective"
      action: "create_file"
      template: "retrospective/template_retrospective.md"
      dest: "retrospective/${current_date}_retrospective.md"
      fill: "${user_answers}"
      message: "振り返りファイルを作成しました。"

    - name: "generate_closing_message"
      action: "generate_message"
      input: "${context}\n${user_answers}"
      style: [ "positive_reflection", "motivational", "healing", "celebratory" ]
      output: "closing_message"

    - name: "complete_day"
      action: "notify"
      message: "${closing_message}"

```