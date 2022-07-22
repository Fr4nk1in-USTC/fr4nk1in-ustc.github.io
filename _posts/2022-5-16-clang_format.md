---
title: 个人 clang-format-15 配置文件
date: 2021-5-16 16:45:00 +0800
categories: [Computer, Tools]
tags: [tips, notes]     # TAG names should always be lowercase
math: false
toc: false
---

# 配置文件
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

# 示例
```c
#include <algorithm>
#include <cstdlib>
#include <ctime>
#include <functional>
#include <iostream>
#include <iterator>

#define BIT_MASK 0xDEADBEAF

#define MULTILINE_DEF(a, b)         \
    if ((a) > 2) {                  \
        auto temp = (b) / 2;        \
        (b)       += 10;            \
        someFunctionCall((a), (b)); \
    }

namespace LevelOneNamespace {
    namespace LevelTwoNamespace {

        template<typename T, int size> bool is_sorted(T (&array)[size])
        {
            return std::adjacent_find(array, array + size, std::greater<T>())
                == array + size;
        }

        std::vector<uint32_t> returnVector(
            uint32_t *LongNameForParameter1, double *LongNameForParameter2,
            const float                          &LongNameForParameter3,
            const std::map<std::string, int32_t> &LongNameForParameter4)
        {
            // TODO: This is a long comment that allows you to understand how
            // long comments will be trimmed. Here should be deep thought but
            // it's just not right time for this

            for (auto &i : LongNameForParameter4) {
                auto b = someFunctionCall(
                    static_cast<int16_t>(*LongNameForParameter2),
                    reinterpret_cast<float *>(LongNameForParameter2));
                i.second++;
            }

            do {
                if (a)
                    a--;
                else
                    a++;
            } while (false);

            return {};
        }

    }  // namespace LevelTwoNamespace
}  // namespace LevelOneNamespace

int main()
{
    std::srand(std::time(0));

    int list[] = {1, 2, 3, 4, 5, 6, 7, 8, 9};

    do {
        std::random_shuffle(list, list + 9);
    } while (is_sorted(list));

    int score = 0;

    do {
        std::cout << "Current list: ";
        std::copy(list, list + 9, std::ostream_iterator<int>(std::cout, " "));

        int rev;
        while (true) {
            std::cout << "\nDigits to reverse? ";
            std::cin >> rev;
            if (rev > 1 && rev < 10)
                break;
            std::cout << "Please enter a value between 2 and 9.";
        }

        ++score;
        std::reverse(list, list + rev);
    } while (!is_sorted(list));

    std::cout << "Congratulations, you sorted the list.\n"
              << "You needed " << score << " reversals." << std::endl;
    return 0;
}

```