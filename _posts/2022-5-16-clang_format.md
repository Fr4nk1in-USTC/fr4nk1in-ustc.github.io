---
title: 个人 clang-format-15 配置文件
date: 2021-5-16 16:45:00 +0800
categories: [Computer, Tools]
tags: [tips, notes]     # TAG names should always be lowercase
math: false
toc: false
---

```yaml
DisableFormat: false
# Tab & Indent
TabWidth: 4
IndentWidth: 4
PPIndentWidth: 4
UseCRLF: false
UseTab: Never

# Configurations
AccessModifierOffset: -2
AlignAfterOpenBracket: Align
AlignArrayOfStructures: Right
AlignConsecutiveAssignments: 
  Enabled: true
  AcrossEmptyLines: false
  AcrossComments: false
  AlignCompound: true
  PadOperators: false
AlignConsecutiveBitFields: 
  Enabled: true
  AcrossEmptyLines: false
  AcrossComments: false
  AlignCompound: true
  PadOperators: false
AlignConsecutiveMacros: 
  Enabled: true
  AcrossEmptyLines: false
  AcrossComments: false
  AlignCompound: true
  PadOperators: false
AlignConsecutiveDeclarations: 
  Enabled: true
  AcrossEmptyLines: false
  AcrossComments: false
  AlignCompound: true
  PadOperators: false
AlignEscapedNewlines: Left
AlignOperands: AlignAfterOperator
AlignTrailingComments: true
AllowAllArgumentsOnNextLine: true
AllowAllParametersOfDeclarationOnNextLine: true
AllowShortBlocksOnASingleLine: Empty
AllowShortCaseLabelsOnASingleLine: false
AllowShortEnumsOnASingleLine: false
AllowShortFunctionsOnASingleLine: Inline
AllowShortIfStatementsOnASingleLine: Never
AllowShortLambdasOnASingleLine: Inline
AllowShortLoopsOnASingleLine: false
AlwaysBreakAfterDefinitionReturnType: None
AlwaysBreakAfterReturnType: None
AlwaysBreakBeforeMultilineStrings: false
AlwaysBreakTemplateDeclarations: MultiLine
BinPackArguments: true
BinPackParameters: true
BitFieldColonSpacing: After
BraceWrapping:
  AfterCaseLabel: false
  AfterClass: true
  AfterControlStatement: MultiLine
  AfterEnum: false
  AfterFunction: true
  AfterNamespace: false
  AfterStruct: false
  AfterUnion: false
  AfterExternBlock: true
  BeforeCatch: false
  BeforeElse: false
  BeforeLambdaBody: false
  BeforeWhile: false
  SplitEmptyFunction: false
  SplitEmptyRecord: false
  SplitEmptyNamespace: false
BreakAfterJavaFieldAnnotations: true
BreakBeforeBinaryOperators: NonAssignment
BreakBeforeBraces: Custom
BreakBeforeConceptDeclarations: true
BreakBeforeTernaryOperators: false
BreakConstructorInitializers: AfterColon
BreakInheritanceList: AfterColon
BreakStringLiterals: true
ColumnLimit: 80
CompactNamespaces: false
ConstructorInitializerIndentWidth: 4
ContinuationIndentWidth: 4
Cpp11BracedListStyle: true
DeriveLineEnding: false
DerivePointerAlignment: false
EmptyLineAfterAccessModifier: Never
EmptyLineBeforeAccessModifier: Always
FixNamespaceComments: true
IncludeBlocks: Regroup
IndentCaseBlocks: true
IndentCaseLabels: false
IndentExternBlock: AfterExternBlock
IndentGotoLabels: false
IndentPPDirectives: BeforeHash
IndentRequiresClause: false
IndentWrappedFunctionNames: true
InsertBraces: false
InsertTrailingCommas: None
KeepEmptyLinesAtTheStartOfBlocks: false
LambdaBodyIndentation: OuterScope
MaxEmptyLinesToKeep: 1
NamespaceIndentation: All
PackConstructorInitializers: CurrentLine
PointerAlignment: Right
ReferenceAlignment: Right
ReflowComments: true
RequiresClausePosition: OwnLine
SeparateDefinitionBlocks: Always
ShortNamespaceLines: 0
SortIncludes: CaseSensitive
SortUsingDeclarations: true
SpaceAfterCStyleCast: false
SpaceAfterLogicalNot: false
SpaceAfterTemplateKeyword: false
SpaceAroundPointerQualifiers: Default
SpaceBeforeAssignmentOperators: true
SpaceBeforeCaseColon: false
SpaceBeforeCpp11BracedList: true
SpaceBeforeCtorInitializerColon: false
SpaceBeforeInheritanceColon: false
SpaceBeforeParens: ControlStatements
SpaceBeforeRangeBasedForLoopColon: true
SpaceBeforeSquareBrackets: false
SpaceInEmptyBlock: false
SpaceInEmptyParentheses: false
SpacesBeforeTrailingComments: 2
SpacesInAngles: Never
SpacesInCStyleCastParentheses: false
SpacesInConditionalStatement: false
SpacesInContainerLiterals: false
SpacesInLineCommentPrefix:
  Minimum: 1
SpacesInParentheses: false
SpacesInSquareBrackets: false
Standard: Auto

# Java
SortJavaStaticImport: Before
```
