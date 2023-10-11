* Install antlr for typescript cli :
```bash
# first install openjdk https://learn.microsoft.com/en-us/java/openjdk/download#openjdk-21
# -- 
# 
#   $ java --version
#   openjdk 18.0.2 2022-07-19
#   OpenJDK Runtime Environment (build 18.0.2+9-61)
#   OpenJDK 64-Bit Server VM (build 18.0.2+9-61, mixed mode, sharing)
# 
# --
npm i -g antlr4ts-cli
java --version
antlr4ts --version
```

* then create a first language compiler :

```bash

export LANG_EXAMPLE_NAME="coolnthe_lang"
mkdir -p ./${LANG_EXAMPLE_NAME}/
cat <<EOF >./${LANG_EXAMPLE_NAME}/${LANG_EXAMPLE_NAME}.g4
grammar ${LANG_EXAMPLE_NAME};

file:          (expr ';' | declaration ';' | assignment ';')+ EOF;
declaration:   VAR IDENTIFIER;
expr:          IDENTIFIER (OP IDENTIFIER)?;
assignment:    IDENTIFIER '=' (NUMBER | expr);
if_statement:  IF '(' expr ')' THEN '{' command '}' (ELSE '{' command '}')?;
command:       STOP | CONTINUE | if_statement | assignment;
IDENTIFIER:    WORD (NUMBER | WORD)*;
IF:            'if';
THEN:          'then';
ELSE:          'else';
OP:            '+' | '-' | '*' | '/';
STOP:          'stop';
CONTINUE:      'continue';
VAR:           'var';

NUMBER:        [1–9]+;
WORD:          [a-z]+;
SPACES:        [\t\r\n ]+ -> skip;
EOF

antlr4ts -visitor -o ./${LANG_EXAMPLE_NAME}/generated ./${LANG_EXAMPLE_NAME}/${LANG_EXAMPLE_NAME}.g4

```

