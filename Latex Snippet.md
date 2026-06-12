[
    // Math mode
 {trigger: "$", replacement: "$$0$", options: "tA"},
 {trigger: "$$", replacement: "$$\n$0\n$", options: "mAw"},
 {trigger: "\begin{", replacement: "\\begin{$0}\n$1\n\\end{$0}", options: "mA"},

 // Latex Workflow
 {trigger: "\\the", replacement: "<b>Theorem $0</b>$1<i>$2</i><br>", options: "tA"},
 {trigger: "\\que", replacement:"<b>Question $0</b><br>", options: "tA"},
 {trigger: "\\ans", replacement:"<b>Answer $0</b><br>", options: "tA"},
 {trigger: "\\con", replacement: "<b>Conjecture $0</b>$1<i>$2</i><br>", options: "tA"},
 {trigger: "\\lem", replacement: "<b>Lemma $0</b>\ <i>$1</i><br>", options: "tA"},
 {trigger: "\\cor", replacement: "<b>Corollary $0</b>\ <i>$1</i><br>", options: "tA"},
 {trigger: "\\def", replacement: "<b>Definition $0</b>$1<br>", options: "tA"},
 {trigger: "\\rem", replacement: "<i>Remark $0.\ </i>\ $1<br>", options: "tA"},
 {trigger: "\\prop", replacement: "<i>Property $0</i><br>", options: "tA"},
 {trigger: "\\proof", replacement: "<i>Proof.\ </i>$0\n $$\\tag*{$\\blacksquare$}$$", options: "tA"},
 {trigger: "\\sproof", replacement: "<i>Sketch of Proof.\ </i>$0\n $$\\tag*{$\\blacksquare$}$$", options: "tA"},
 {trigger: "\\begin{equation} ", replacement: "\\begin{equation}{\tag$0}", options: "mA"},
 {trigger: "\\refth", replacement: "[[^$0|Theorem $0]]", options: "tA"},
 {trigger: "\\refeq", replacement: "[[^\\tag{$0|eq. ($0)]]", options: "tA"},
 {trigger: "\\refle", replacement: "[[^$0|Lemma $0]]", options: "tA"},
 {trigger: "\\refco", replacement: "[[^$0|Corollary $0]]", options: "tA"},
 {trigger: "\\refde", replacement: "[[^$0|Definition $0]]", options: "tA"},
 {trigger: "\\refre", replacement: "[[^$0|Remark $0]]", options: "tA"},

 //Completion
 {trigger: "(", replacement: "(${VISUAL})", options: "mA"},
 {trigger: "[", replacement: "[${VISUAL}]", options: "mA"},
 {trigger: "{", replacement: "{${VISUAL}}", options: "mA"},
 {trigger: "(", replacement: "($0)$1", options: "mA"},
 {trigger: "{", replacement: "{$0}$1", options: "mA"},
 {trigger: "[", replacement: "[$0]$1", options: "mA"},
 {trigger: "\\(", replacement: "\\left( $0 \\right) $1", options: "mA"},
 {trigger: "\\{", replacement: "\\left\\{ $0 \\right\\} $1", options: "mA"},
 {trigger: "\\[", replacement: "\\left[ $0 \\right] $1", options: "mA"},
 {trigger: "\\|", replacement: "\\left| $0 \\right| $1", options: "mA"},
 {trigger: "\\<", replacement: "\\left< $0 \\right> $1", options: "mA"},
 {trigger: "\\sum", replacement: "\\sum_{${0:i}=${1:1}}^{${2:N}} $3", options: "mA"},
 {trigger: "\\prod", replacement: "\\prod_{${0:i}=${1:1}}^{${2:N}} $3", options: "mA"},
 {trigger: "\\lim", replacement: "\\lim_{ ${0:n} \\to ${1:\\infty} } $2", options: "mA"},
 {trigger: "+-", replacement: "\\pm", options: "mA"},
 {trigger: "-+", replacement: "\\mp", options: "mA"},
 {trigger: "\\begin\{", replacement: "\\begin{$0}\n$1\n\\end{$0}", options: "mA"},

 //Markdown Shortcuts
 {trigger: "\\che", replacement: "- [ ]", options: "tA"},

 //Tikz-cd 
 {trigger: "\\tikzcd", replacement: "```latex\n\\usepackage{tikz-cd}\n\\begin{document}\n\\begin{tikzcd}[color=white!100]\n $0 \n\\end{tikzcd}\n\\end{document}\n```", options: "tA"},

 //Title
 {trigger: "\\maketitle", replacement: "<div class=\"latex-title-block\">\n<h1 class=\"title\">$0</h1>\n<div class=\"author\">$1</div>\n<div class=\"date\">$2</div>\n</div>", options: "tA"},
]
