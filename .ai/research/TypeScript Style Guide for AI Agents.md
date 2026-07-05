# **Mapped Architectural Terrain for AI Coding Agents: TypeScript 6, Bun, and Biome Style Guide**

Modern runtime environments have shifted the web development ecosystem toward an execution model centered on pure type erasure and high-speed native toolchains.1 This architectural report maps the technical terrain of a custom TypeScript 6 style guide designed to be ingested as a system instruction skill by artificial intelligence coding agents.3 By providing deterministic configuration and style rules, this guide ensures that coding agents write highly performant, predictable code within a unified toolchain powered by Bun for runtime execution and package management, Biome for linting and formatting, and Vite for client-side bundler integration.5  
The upcoming release of the Go-based TypeScript 7 compiler demands that contemporary codebases proactively eliminate legacy configurations and non-erasable language features.1 Consequently, this style guide defines exactly thirty-five core topics, each structured to form a single markdown instruction page in the final agent skill repository.3 For every topic, this report details the under-the-hood compiler mechanics and defines the explicit strategic decisions that must be established during the drafting phase to guarantee alignment with modern type-stripping runtimes.1

## **Technical Landscape and Mapped Style Guide Terrain**

The following structural matrices map out the exact topics, configurations, and toolchain choices that form the core of the style guide. These tables represent the technical standards that the AI agent will refer to when writing code.

### **Mapped Style Guide Terrain (Topics 1–35 Index)**

| Topic Number | Category | Style Guide Topic Name | Core Mechanism / Target Feature | Core Choice / Option Framework |
| :---- | :---- | :---- | :---- | :---- |
| **1** | Configuration | Workspace Root Directory and Monorepos | rootDir and outDir alignment 12 | Set explicit source paths vs. allow dynamic default resolution.12 |
| **2** | Configuration | Strict Mode Compilation Strategy | strict: true default compiler options 14 | Enforce globally vs. allow localized opt-outs per package.12 |
| **3** | Configuration | Global Type Resolution and Empty Array | types: optimization 12 | Explicitly define required global types vs. use wildcard imports.17 |
| **4** | Configuration | Module Emit and Legacy Deprecations | Deprecation of ES5 and Downlevel Iteration 1 | Enforce immediate modern syntax vs. use temporary suppressions.19 |
| **5** | Configuration | Target Version and Evergreen Runtimes | Floating target: "es2025" mechanics 14 | Align to floating modern specs vs. pin to a stable fixed version.21 |
| **6** | Configuration | Compiler Options for Library Auditing | libReplacement: false optimization 15 | Enforce compile-time performance wins vs. maintain custom libs.19 |
| **7** | Runtime Syntax | Strict Type Erasure Enforcements | erasableSyntaxOnly compiler validations 10 | Mandate pure erasable syntax globally vs. allow local transforms.10 |
| **8** | Runtime Syntax | Standardizing Type Imports and Exports | verbatimModuleSyntax and import elision 25 | Enforce strict separated type imports vs. permit inline syntax.11 |
| **9** | Runtime Syntax | Relocating Aliases to Package Imports | package.json\#imports subpath mappings 10 | Force native \# imports vs. retain historical paths aliases.10 |
| **10** | Runtime Syntax | Alternatives to Namespace Merging | Namespace removal and ES modules 1 | Complete ban on runtime namespaces vs. compile with transforms.2 |
| **11** | Runtime Syntax | Migration of Enums to Read-Only Objects | Banning enums in favor of as const 10 | Convert strictly to const-asserted objects vs. support transforms.1 |
| **12** | Runtime Syntax | Eliminating Class Parameter Properties | Field declarations in constructor signatures 10 | Require standard properties vs. permit parameter properties.2 |
| **13** | Modern APIs | Safe Date and Time Engineering | TC39 Temporal API integration 14 | Completely ban the legacy Date object vs. permit coexistence.14 |
| **14** | Modern APIs | Boilerplate-Free Map Operations | Map getOrInsert and computed upserts 14 | Enforce native upsert methods vs. allow legacy checking loops.19 |
| **15** | Modern APIs | Safe Dynamic String Escaping | RegExp.escape for dynamic inputs 14 | Mandate native RegExp escaping vs. use external custom libraries.14 |
| **16** | Modern APIs | Unified Error Control Flows | Promise.try for sync/async boundaries 14 | Unify errors via native Promise.try vs. traditional try-catch. |
| **17** | Modern APIs | Declarative Resource Management | using keyword and Symbol.dispose 8 | Mandate using-declarations globally vs. permit manual cleanup.27 |
| **18** | Modern APIs | Asynchronous Resource Disposal | await using and Symbol.asyncDispose 27 | Enforce async resource protocols vs. resolve with manual promises.27 |
| **19** | Modern APIs | Eliminating Ambiguous Conditionals | Redundant truthiness and nullish checks 32 | Enforce strict boolean verification vs. accept implicit evaluations. |
| **20** | Bun Runtime | Bun-Native TypeScript Execution | Type-stripping compilation runtime 6 | Enforce strict pre-execution checks vs. rely on compiler watch.6 |
| **21** | Bun Runtime | Decoupled Dev Loop Type Verification | tsc \--noEmit validation patterns 6 | Execute type checking during pre-commit hooks vs. run in CI only.6 |
| **22** | Bun Runtime | Global Bun API Declarations | @types/bun environment typings 6 | Register types globally across workspace vs. isolate to backend.17 |
| **23** | Bun Runtime | Project References and Monorepos | composite: true monorepo links 21 | Enforce strict project references vs. rely on workspace links.36 |
| **24** | Bun Runtime | Bun Shell Scripting and Automation | Native platform-agnostic automation 33 | Mandate native Bun Shell scripting vs. support standard Node tasks.6 |
| **25** | Bun Runtime | Side-Effect Imports and Asset Mapping | noUncheckedSideEffectImports defaults 15 | Map asset paths via wildcard declarations vs. disable checking.38 |
| **26** | Biome Toolchain | Biome Layout and Formatting Strategy | Formatting rules and spacing parameters 40 | Adopt default Biome style (tabs) vs. apply custom overrides.41 |
| **27** | Biome Toolchain | Auto-Fix Severity and Remediation | Linter diagnostics and code adjustments 43 | Limit save actions to safe fixes vs. allow unsafe modifications.43 |
| **28** | Biome Toolchain | Type Inference and Static Heuristics | Biome 2.0+ type-aware validations 5 | Depend entirely on Biome’s parser vs. keep ESLint for CI checks.5 |
| **29** | Biome Toolchain | Strict Casing and System Naming | useNamingConvention variable rules 44 | Enforce strict naming conventions globally vs. allow external snake\_case.41 |
| **30** | Biome Toolchain | Safe Logic Operators and Comparisons | Strict equality checks and null comparisons 45 | Enforce absolute strict equality vs. allow loose null checks.42 |
| **31** | Biome Toolchain | Functional Collections Optimization | Iteration loops and collection operations 45 | Force strict loop mappings globally vs. permit standard iterations.47 |
| **32** | Biome Toolchain | Cognitive Complexity Restraints | noExcessiveCognitiveComplexity parameters 45 | Reject commits exceeding complexity limits vs. log as warnings.45 |
| **33** | Client Integration | Client HMR and Fast Refresh | Vite bundler compilation workflows 7 | Run background type-checkers on save vs. isolate in build phase.10 |
| **34** | Testing / Mocks | High-Performance Test Orchestration | Bun Test runner execution patterns 35 | Run all packages under Bun Test vs. use Vitest for client.34 |
| **35** | Testing / Mocks | Module-Level Mocking Strategy | mock.module and module preload utilities 50 | Declare mocks in centralized preloader scripts vs. write inline.50 |

## **Mapped Configuration and Workspace Terrain**

Setting up a robust development environment requires aligning the compiler options inside tsconfig.json with modern runtime defaults. TypeScript 6 updates compiler behaviors to improve build efficiency and match the standards of evergreen execution targets.12

### **TypeScript 6 Breaking Changes and Configuration Migration**

| Compiler Flag | TS 5.x Default Behavior | TS 6.0 Default Behavior | Strategic Migration Remediation |
| :---- | :---- | :---- | :---- |
| strict | false (requires manual enablement) | true | Address implicit any and unhandled null types directly in legacy codebases.12 |
| types | Crawls all packages in node\_modules/@types | \`\` (empty array; no auto-discovery) | Explicitly define required globals (e.g., \["bun", "react"\]) per workspace package.17 |
| rootDir | Inferred dynamically from common source paths | Path containing the local tsconfig.json | Explicitly define "rootDir": "./src" to prevent nested output layouts in build pipelines.12 |
| module | Varies based on environmental presets | esnext | Migrate build targets and import syntax strictly to modern ECMAScript Modules (ESM).12 |
| target | Varies (often legacy standards like es5) | es2025 (floats with current spec) | Stop using downlevel transformations; target modern, evergreen runtimes natively.12 |
| noUncheckedSideEffectImports | false (missing non-JS files are ignored) | true | Declare wildcard module definitions for assets like .css and .svg inside global declaration files.38 |

### **Mapped Terrain: 35 Core Style Guide Topics**

#### **1\. Workspace Root Directory and Multi-package Monorepos**

In previous iterations of the language service, the compiler silently inferred the common root directory of source files to structure output builds.12 TypeScript 6 alters this behavior by defaulting rootDir to the directory containing the local tsconfig.json.12 If an application uses a nested layout (such as source code located in a ./src folder) without an explicit rootDir directive, the build system will output files into a nested directory structure, such as ./dist/src/index.js.13

* **Decision Point**: The style guide must decide whether to mandate that every package explicitly define "rootDir": "./src" inside its local configuration to maintain backward compatibility, or reform the downstream build pipeline to handle output layout variations natively.12

#### **2\. Strict Mode Compilation Strategy**

With strict mode now active by default in TypeScript 6, coding agents must address strict null checks, function parameter validations, and binding assignments without depending on manual environment configurations.6 This default change requires resolving unhandled null values and implicit any types directly at the source-code level.12

* **Decision Point**: The style guide must choose between Option A, which enforces strict compliance across all packages with no overrides, or Option B, which permits localized opt-outs (such as setting strictNullChecks: false) within specific directories using localized configuration files during gradual code migrations.12

#### **3\. Global Type Resolution and the Empty Types Array**

TypeScript 6 defaults the types array to \`\`, meaning the compiler no longer crawls the node\_modules/@types directory automatically.12 This optimization provides a 20% to 50% reduction in compilation times by eliminating unnecessary ambient declaration lookups.12 However, this change will trigger errors in modules that rely on implicit global variables such as process or \_\_dirname.15

* **Decision Point**: The style guide must choose between Option A, which mandates that each package explicitly list its required globals (e.g., types: \["bun"\] or types: \["react"\]) in its local configuration, or Option B, which restores the legacy behavior by setting a root override of types: \["\*"\].15

#### **4\. Module Emit and Legacy Deprecations**

To streamline the compiler and prepare for the native Go-based performance improvements in TypeScript 7, several older configuration options are now deprecated in TypeScript 6\.1 These include compiling to legacy ES5 targets, using \--downlevelIteration transforms, and relying on classic Node10 module resolution.12

* **Decision Point**: The style guide must choose between Option A, which requires immediate compliance with modern target and resolution standards, or Option B, which permits the use of "ignoreDeprecations": "6.0" to temporarily bypass build errors during transition periods.1

#### **5\. Target Version and Evergreen Runtimes**

The default compiler target in TypeScript 6 is set to the current-year ECMAScript standard (currently es2025).12 This setup allows the compiler to omit legacy downlevel transformations, since modern execution environments natively support newer language features.12

* **Decision Point**: The style guide must choose between Option A, which adopts this floating standard to automatically utilize new ECMAScript capabilities as they release, or Option B, which locks the compilation target to a stable, fixed standard (such as es2022) to guarantee absolute environment compatibility.21

#### **6\. Compiler Options for Library Auditing**

TypeScript 6 introduces the libReplacement: false default, improving compilation performance by preventing duplicate module resolution loops when auditing internal definitions.15 This prevents the compiler from running redundant watches over standard declarations unless they are explicitly changed.15

* **Decision Point**: The style guide must choose between Option A, which leaves this optimization enabled globally to maximize compilation speed, or Option B, which overrides this default to libReplacement: true in legacy monorepos that require custom ambient declaration overrides.15

#### **7\. Strict Type Erasure Enforcements**

Under the \--erasableSyntaxOnly compiler option, TypeScript throws errors for features that require compiler-generated runtime code, such as enums, namespaces containing executable statements, class constructor parameter properties, and legacy import/export assignments.10 This flag ensures that source files can be safely executed by lightweight, type-stripping runtimes like Bun by treating type annotations purely as comments.6

* **Decision Point**: The style guide must choose between Option A, which mandates that erasableSyntaxOnly is set to true globally to guarantee immediate runtime compatibility, or Option B, which disables the option and relies on build-step transpilation tools to process non-erasable language structures.1

#### **8\. Standardizing Type Imports and Exports**

The verbatimModuleSyntax option enforces a strict separation between value imports and type-only imports.11 When this option is active, any dependency used solely for its type definitions must use the import type syntax.11 This ensures that type-only imports are cleanly erased during compilation, preventing runtimes from trying to resolve modules that do not exist at runtime.11

* **Decision Point**: The style guide must choose between Option A, which requires strict segregation by banning combined value-and-type imports, or Option B, which permits mixed import declarations (e.g., import { MyClass, type MyType } from "./module") where the toolchain is guaranteed to handle inline type stripping safely.11

#### **9\. Relocating Module Aliases to Package Imports**

Traditional TypeScript import paths mapped via the paths compiler option operate solely as compile-time helpers and are ignored during runtime resolution.10 TypeScript 6 recommends replacing the paths config with the standard Node-native package.json\#imports field.10 This native mechanism requires prefixing import aliases with a \# character (e.g., \#utils/format) and is supported out of the box by both Node and Bun.10

* **Decision Point**: The style guide must choose between Option A, which mandates transitioning entirely to native package.json\#imports and removing the paths config from tsconfig.json 10, or Option B, which retains classic paths alias mapping to preserve legacy workspace compatibility.10

#### **10\. Alternatives to TypeScript Namespace Merging**

Because namespaces containing executable statements emit runtime code, they are flagged as compilation errors when working in erasable syntax environments.10 To keep files fully erasable, namespaces must be refactored into standard ES modules, utilizing explicit export statements to manage module scope natively.1

* **Decision Point**: The style guide must choose between Option A, which bans the use of runtime namespaces across the entire codebase 1, or Option B, which permits namespaces only within isolated packages that compile through custom transpilation filters.1

#### **11\. Migration of Native Enums to Read-Only Const Objects**

Standard TypeScript enums do not map to valid JavaScript and emit a complex runtime object structure instead.2 This generation pattern violates the principles of type erasure.10 To replace enums, the style guide should recommend using const-asserted objects combined with union types, which are cleanly erased during build steps.10

TypeScript  
// Enforced modern pattern replacing native enums  
const Direction \= {  
  Up: "Up",  
  Down: "Down",  
  Left: "Left",  
  Right: "Right",  
} as const;

type Direction \= typeof Direction;

* **Decision Point**: The style guide must choose between Option A, which enforces this modern as const object pattern globally 10, or Option B, which permits native enums under a custom transpilation step at the expense of native runtime execution.1

#### **12\. Eliminating Constructor Class Parameter Properties**

Declaring properties directly within a constructor parameter signature (e.g., constructor(private x: number)) forces the compiler to generate actual variable assignment code at runtime.2 This behavior is prohibited under strict type-erasure rules.10 Instead, variables must be declared explicitly as class properties and assigned their values within a standard constructor block.10

TypeScript  
// Enforced erasable class property initialization pattern  
class Point {  
  public x: number;  
  public y: number;

  constructor(x: number, y: number) {  
    this.x \= x;  
    this.y \= y;  
  }  
}

* **Decision Point**: The style guide must choose between Option A, which mandates standard property declarations for all classes 10, or Option B, which permits parameter properties in specific backend targets that compile through custom transpiler pipelines.2

#### **13\. Safe Date and Time Engineering with the Temporal API**

The mutable design of the legacy Date object has historically been a source of subtle timezone and arithmetic bugs.14 TypeScript 6 ships with built-in type support for the TC39 Temporal API, allowing safe, immutable, timezone-aware date and time manipulations.14

* **Decision Point**: The style guide must choose between Option A, which strictly bans the legacy Date object in new codebases and mandates using the Temporal API 14, or Option B, which permits the use of both date interfaces to simplify integrations with external systems.27

#### **14\. Boilerplate-Free Map Operations via Upsert APIs**

To simplify map updates, TypeScript 6 includes type definitions for the native Map methods getOrInsert and getOrInsertComputed.14 These methods look up a key and automatically insert a default value if the key is missing, replacing traditional check-and-set loops.19

* **Decision Point**: The style guide must choose between Option A, which mandates using these native upsert APIs to reduce boilerplate 14, or Option B, which permits traditional map lookups to maintain compatibility with runtimes that lack native upsert support.19

#### **15\. Safe Dynamic String Escaping in Regular Expressions**

Building regular expressions from dynamic string inputs has historically required writing manual, error-prone escape utilities.14 TypeScript 6 supports the native RegExp.escape method, ensuring that dynamic inputs are safely escaped before constructing regex objects.14

* **Decision Point**: The style guide must choose between Option A, which mandates using RegExp.escape for all variable inputs to RegExp constructors 14, or Option B, which permits custom string-escaping libraries where specific, non-standard behaviors are required.

#### **16\. Unified Synchronous and Asynchronous Error Control**

Handling errors across mixed synchronous and asynchronous code paths often requires nesting try-catch blocks and promise chains.14 The native Promise.try API provides a unified error-handling flow by wrapping both synchronous and asynchronous functions in a single, type-safe promise chain.14

* **Decision Point**: The style guide must choose between Option A, which mandates using Promise.try to wrap complex, multi-step pipelines 14, or Option B, which permits traditional try-catch blocks to keep local operations lightweight.

#### **17\. Declarative Resource Management and Scope Termination**

To make resource cleanup safer, TypeScript 6 supports the native ECMAScript using declaration.8 When a variable is declared with the using keyword, its Symbol.dispose method is automatically called when execution leaves the enclosing block, avoiding the need for manual try-finally cleanup blocks.27

TypeScript  
// Enforced automatic resource management pattern  
class DatabaseConnection implements Disposable {  
  public query(sql: string) {  
    // execute query  
  }

 () {  
    // release connection back to pool  
  }  
}

function processData() {  
  using db \= new DatabaseConnection();  
  db.query("SELECT 1"); // connection is automatically released when function exits  
}

* **Decision Point**: The style guide must choose between Option A, which mandates using the using keyword for all system resources (such as file handles, database connections, and locks) 27, or Option B, which permits standard try-finally blocks to keep simple functions lightweight.29

#### **18\. Asynchronous Resource Disposal Protocols**

For resources that require asynchronous cleanup, TypeScript 6 supports await using declarations.27 These declare variables whose asynchronous cleanup methods (keyed by Symbol.asyncDispose) are automatically called and awaited when execution leaves the current scope.27

* **Decision Point**: The style guide must choose between Option A, which mandates using await using for all asynchronous resources 27, or Option B, which permits manual promise chaining to avoid introducing async blocks into synchronous paths.29

#### **19\. Eliminating Ambiguous Conditionals and Truthiness Checks**

TypeScript 6 flags potential logical errors by warning developers about conditional expressions that will always evaluate to true or false (such as \`const value \= {} |  
| 'unreachable'\`).32 This prevents dead code from remaining in logical branches.32

* **Decision Point**: The style guide must choose between Option A, which treats these redundant logical checks as build-blocking errors 32, or Option B, which logs them as warnings to allow for rapid debugging during development.

#### **20\. Bun-Native TypeScript Execution and Type-Stripping**

Bun executes TypeScript files natively by stripping type annotations on the fly without running a separate compilation step.6 While this approach is incredibly fast, it means Bun does not perform type checking during runtime execution.6

* **Decision Point**: The style guide must choose between Option A, which requires type-checking files via pre-execution tasks in development runners 6, or Option B, which allows fast local runs without concurrent checking, relying entirely on real-time editor diagnostics to catch errors.6

#### **21\. Decoupled Dev Loop Type Verification**

Since Bun's runtime transpiler bypasses type checking to maximize performance, type safety must be verified in a separate step.6 The standard way to verify types without building files is to run the compiler in non-emit mode: bunx tsc \--noEmit.6

* **Decision Point**: The style guide must choose between Option A, which runs bunx tsc \--noEmit locally during pre-commit or pre-push hooks 6, or Option B, which defers type verification entirely to continuous integration (CI) environments to avoid slowing down local git operations.6

#### **22\. Global Bun API Declarations and Autocomplete**

To prevent the compiler from generating errors when accessing Bun-specific globals (e.g., Bun.serve), the project must register Bun's global type definitions.6 This is done by installing @types/bun as a devDependency and referencing it in the compiler configuration.6

* **Decision Point**: The style guide must choose between Option A, which registers Bun's types globally in the root tsconfig.json 17, or Option B, which registers Bun's types only within backend subdirectories to prevent global pollution in client-facing packages.18

#### **23\. TypeScript Project References and Monorepos**

In multi-package monorepos, keeping compilation targets aligned can become complex.37 TypeScript project references (enabled via composite: true) allow packages to build and cache incrementally, keeping project relationships clean.21

* **Decision Point**: The style guide must choose between Option A, which mandates using strict project references and building via tsc \--build 37, or Option B, which relies on Bun's workspace linking to resolve packages without managing composite structures.34

#### **24\. Bun Shell Scripting and Automation Task Conventions**

Bun Shell provides a cross-platform, high-performance scripting utility, allowing Unix-like shell syntax to run identically across all platforms (including Windows).33 This provides a reliable, fast way to write platform-agnostic automation scripts.33

* **Decision Point**: The style guide must choose between Option A, which mandates using the Bun Shell for all custom build and deployment tasks 33, or Option B, which maintains traditional Node compatibility flags to allow tasks to run in legacy environments.6

#### **25\. Side-Effect Imports and Ambient Asset Declarations**

TypeScript 6 enables noUncheckedSideEffectImports by default, generating errors if side-effect imports (such as importing a CSS file: import "./styles.css") cannot be resolved to a physical file.15

* **Decision Point**: The style guide must choose between Option A, which keeps the strict default and resolves unresolved asset imports using wildcard ambient declarations (e.g., declare module "\*.css" {}) in a global .d.ts file 38, or Option B, which sets noUncheckedSideEffectImports: false to bypass path checking.38

### **Mapped Biome Linter and Style Terrain**

Biome replaces traditional, complex configurations (like ESLint and Prettier) with a unified, high-performance toolchain written in Rust.5 Biome compiles rules in-tree, providing immediate, local feedback while eliminating the need to coordinate multiple packages and plugins.5

### **Architectural Trade-offs: Biome Linter vs. ESLint Toolchain**

| Capability Dimension | Biome Toolchain | ESLint \+ Prettier (Legacy) | Strategic Performance / Coverage Impact |
| :---- | :---- | :---- | :---- |
| **Execution Performance** | 1–3 seconds per 50,000 lines of code.41 | 15–30 seconds per 50,000 lines of code.52 | Biome's native, Rust-based engine dramatically cuts local lint times and reduces CI pipeline runtimes.52 |
| **Type-Aware Depth** | Heuristic static analysis (\~85% coverage).5 | Full integration with the TypeScript compiler.5 | ESLint catches deep type-checker issues at the cost of being 2x to 3x slower during development.5 |
| **Configuration Profile** | Single, self-contained biome.json file.5 | Fragmented files, plugins, and dependencies.5 | Biome completely eliminates configuration sprawl and prevents conflicts between formatting and linting rules.5 |
| **Ecosystem Extensibility** | Curated, in-tree rule set; no custom plugins.5 | Over 1,000 custom plugins.5 | Biome trades third-party plugin depth for absolute stability and zero dependency maintenance.5 |

#### **26\. Biome Layout and Formatting Strategy**

Biome's formatter is designed to format code automatically on save, keeping styles consistent across the entire project.34 The formatter handles formatting directly in Rust, completely eliminating potential style conflicts between the linter and formatter.40

* **Decision Point**: The style guide must choose between Option A, which adopts Biome's default formatting values (such as double quotes and tab-based indentation) 40, or Option B, which applies custom style overrides (such as spaces and single quotes) to match legacy project standards.41

#### **27\. Auto-Fix Severity and Safe Lint Remediation**

Biome categorizes code fixes as either safe or unsafe.43 Safe fixes are guaranteed not to alter the runtime semantics of the program, whereas unsafe fixes may change runtime behavior and require manual review.43

* **Decision Point**: The style guide must choose between Option A, which limits save actions to safe fixes only 43, or Option B, which permits auto-applying unsafe fixes in non-critical environments to speed up development.43

#### **28\. Type Inference and Static Heuristics in Biome**

While Biome 2.0+ does not integrate with tsconfig.json directly, it uses high-performance static heuristics to catch common type-related issues (such as unhandled promises: noFloatingPromises).5 This provides fast feedback on typical bugs without the performance penalty of running the full TypeScript compiler.5

* **Decision Point**: The style guide must choose between Option A, which relies entirely on Biome's static checks 5, or Option B, which keeps ESLint in the CI pipeline exclusively to run deeper, type-aware checks.5

#### **29\. Strict Casing and System Naming Conventions**

Biome includes the useNamingConvention style rule, allowing projects to enforce consistent casing rules across variables, functions, classes, and properties.41

* **Decision Point**: The style guide must choose between Option A, which enforces strict casing rules globally (e.g., camelCase for variables and PascalCase for classes) 44, or Option B, which disables strict casing checks to accommodate external snake\_case APIs.41

#### **30\. Safe Logic Operators and Strict Comparisons**

The noDoubleEquals rule prevents developers from using loose comparisons (==), which can lead to unexpected type coercions.42 It requires using strict equality (===) instead, while allowing loose comparisons only when checking against nullish values to safely match both null and undefined.42

* **Decision Point**: The style guide must choose between Option A, which mandates strict triple equals (===) for all comparisons, banning double equals entirely 45, or Option B, which permits double equals checks exclusively when comparing against null.42

#### **31\. Array Traversal, Functional Collections, and Performance Optimizations**

Biome includes style rules like noForEach (which recommends for...of loops over Array.prototype.forEach) and useFlatMap (which recommends .flatMap() over .map().flat()) to keep array operations fast and clean.41

* **Decision Point**: The style guide must choose between Option A, which enforces noForEach globally to optimize iteration performance 47, or Option B, which permits using .forEach() where it may be more readable.47

#### **32\. Control of Cognitive Complexity and Nesting Thresholds**

The noExcessiveCognitiveComplexity rule calculates a function's cognitive complexity mathematically based on nesting levels and logical conditions.45 This prevents codebases from accumulating overly complex, hard-to-test functions.45

TypeScript  
// Enforced cognitive complexity limit  
// Let Cc(f) represent the cognitive complexity of function f:  
// Cc(f) \<= 15  
function processItems(items: string) {  
  if (items.length \=== 0) return; // Cc \+ 1

  for (const item of items) { // Cc \+ 1 (nesting level 1\)  
    if (item.trim()\!== "") { // Cc \+ 2 (nesting level 2\)  
      console.log(item);  
    }  
  }  
}

* **Decision Point**: The style guide must choose between Option A, which treats complexity violations as build-blocking errors 47, or Option B, which logs violations as warnings to avoid interrupting development flows.41

### **Mapped Bundler, Testing, and Mocking Terrain**

Integrating client-side tools and testing framework interfaces ensures that developers have a fast, reliable environment for writing and verifying code.7

#### **33\. Client-Side Hot Module Replacement and Fast Refresh with Vite**

For client-side applications (such as React), Vite uses ES modules to compile and serve code extremely fast during development, providing near-instant Hot Module Replacement (HMR).7 Vite compiles code using esbuild, which operates on a single-file basis and strips type annotations without performing full type checks.24

* **Decision Point**: The style guide must choose between Option A, which runs a background type-checking process (tsc \--watch) alongside Vite's development server 10, or Option B, which disables concurrent type checking during development to maximize HMR speed, deferring type verification to build or CI steps.6

#### **34\. High-Performance Test Orchestration with Bun Test**

Bun includes a native, high-performance test runner (bun:test) that provides full compatibility with standard Jest testing APIs.35 The runner supports TypeScript and JSX natively with zero additional configuration, running tests significantly faster than legacy test environments.34

TypeScript  
// Standard Bun Test suite template  
import { describe, test, expect, mock } from "bun:test";

describe("math operations", () \=\> {  
  test("addition", () \=\> {  
    expect(2 \+ 2).toBe(4);  
  });  
});

* **Decision Point**: The style guide must choose between Option A, which standardizes on bun:test for both backend and client-side testing 34, or Option B, which uses bun:test for backend services and Vitest for frontend units to maintain access to legacy DOM testing ecosystems.34

#### **35\. Module-Level Mocking Strategy**

The Bun test runner provides the mock.module() API to dynamically override imports and inject mocked implementations directly into the module cache.50 This is essential for keeping tests isolated and fast.35

* **Decision Point**: The style guide must choose between Option A, which requires all module mocks to be registered inside a centralized preload script 50, or Option B, which permits declaring inline module mocks within individual test files to keep tests self-contained.50

### **Bun-Native Testing, Mocking, and Assertion API Matrix**

The Bun test runner (bun:test) provides a high-performance, Jest-compatible API to manage mock behavior and clear state during test cycles.35

| Testing Subsystem | Native API Interface | Target Behavior | Workspace Verification Paradigm |
| :---- | :---- | :---- | :---- |
| **Mock Creation** | const fn \= mock((x) \=\> x \* 2); | Instantiates a tracked mock function.50 | Use to mock callbacks and verify execution parameters.50 |
| **Spying** | const spy \= spyOn(obj, "method"); | Spies on an existing object method.50 | Track call histories without altering original implementations.50 |
| **Module Mocking** | mock.module("./module", () \=\> ({ x: 1 })); | Overrides a module's exports globally.50 | Mock external network or filesystem dependencies cleanly.50 |
| **Mock Clearing** | mockFn.mockClear(); | Clears call history.50 | Run in beforeEach() blocks to isolate tests.35 |
| **Mock Resetting** | mockFn.mockReset(); | Clears history and removes mock implementations.50 | Restore original method behaviors between tests.35 |

## **Actionable Recommendations for Skill Packaging**

To successfully compile these 35 topics into a cohesive, system-level markdown skill that AI coding agents can consult, the final drafting phase should follow these structural steps:

* **Structure Each Topic as a Single Page**: To avoid context bloat and ensure fast lookups, each of the 35 topics must be formatted as a single, self-contained markdown page inside the agent skill directory.4  
* **Keep Explanations Short and Objective**: Write rules as clear, direct statements of style and configuration.4 Skip redundant explanations of common rules that are already enforced by Biome's formatter or linter.4  
* **Show Clear Preferred and Avoided Patterns**: AI agents respond much better to concrete code examples than to abstract style descriptions.4 Ensure every topic page includes a "Preferred Pattern" and an "Avoided Pattern" code block.4  
* **Provide the Under-the-Hood Architectural Context**: Explain the performance and compatibility reasons behind each rule.4 Knowing *why* a rule exists (such as avoiding non-erasable enums to keep type stripping fast) helps the agent make better styling decisions in complex edge cases.2

#### **Works cited**

1. State of TypeScript 2026 | The Dev Newsletter, accessed July 4, 2026, [https://devnewsletter.com/p/state-of-typescript-2026/](https://devnewsletter.com/p/state-of-typescript-2026/)  
2. TypeScript's \`erasableSyntaxOnly\` Flag \- oida.dev, accessed July 4, 2026, [https://oida.dev/erasable-syntax-only/](https://oida.dev/erasable-syntax-only/)  
3. The SKILL.md Pattern: How to Write AI Agent Skills That Actually Work | by Bibek Poudel, accessed July 4, 2026, [https://bibek-poudel.medium.com/the-skill-md-pattern-how-to-write-ai-agent-skills-that-actually-work-72a3169dd7ee](https://bibek-poudel.medium.com/the-skill-md-pattern-how-to-write-ai-agent-skills-that-actually-work-72a3169dd7ee)  
4. Use custom instructions in VS Code, accessed July 4, 2026, [https://code.visualstudio.com/docs/agent-customization/custom-instructions](https://code.visualstudio.com/docs/agent-customization/custom-instructions)  
5. Biome vs ESLint: Comparing JavaScript Linters and Formatters ..., accessed July 4, 2026, [https://betterstack.com/community/guides/scaling-nodejs/biome-eslint/](https://betterstack.com/community/guides/scaling-nodejs/biome-eslint/)  
6. How to Use TypeScript with Bun \- OneUptime, accessed July 4, 2026, [https://oneuptime.com/blog/post/2026-01-31-bun-typescript/view](https://oneuptime.com/blog/post/2026-01-31-bun-typescript/view)  
7. Hono \+ Inertia \+ React Setup — Bun / Vite / TypeScript Bootstrapped in 30 Minutes \[2026\], accessed July 4, 2026, [https://www.oflight.co.jp/en/columns/hono-inertia-react-setup-bun-vite-2026](https://www.oflight.co.jp/en/columns/hono-inertia-react-setup-bun-vite-2026)  
8. TypeScript 6.0 Released: The Last JavaScript-Based Version — New Features, Breaking Changes, and Migration Guide | jsmanifest, accessed July 4, 2026, [https://jsmanifest.com/typescript-6-final-javascript-release](https://jsmanifest.com/typescript-6-final-javascript-release)  
9. TypeScript 6.0 Ships as Final JavaScript-Based Release, Clears Path for Go-Native 7.0, accessed July 4, 2026, [https://visualstudiomagazine.com/articles/2026/03/23/typescript-6-0-ships-as-final-javascript-based-release-clears-path-for-go-native-7-0.aspx](https://visualstudiomagazine.com/articles/2026/03/23/typescript-6-0-ships-as-final-javascript-based-release-clears-path-for-go-native-7-0.aspx)  
10. TypeScript 5.8 Ships \--erasableSyntaxOnly To Disable Enums ..., accessed July 4, 2026, [https://www.totaltypescript.com/erasable-syntax-only](https://www.totaltypescript.com/erasable-syntax-only)  
11. Modules: TypeScript | Node.js v26.4.0 Documentation, accessed July 4, 2026, [https://nodejs.org/api/typescript.html](https://nodejs.org/api/typescript.html)  
12. TypeScript 6.0: what actually changed, what will break after the upgrade, and whether you should move now | PAS7 STUDIO, accessed July 4, 2026, [https://pas7.com.ua/blog/en/typescript-6-explained-2026](https://pas7.com.ua/blog/en/typescript-6-explained-2026)  
13. TypeScript 6.0 Will Break Your Build (Checklist Saved Me Hours For Upgrading 3 Projects), accessed July 4, 2026, [https://thinkingthroughcode.medium.com/typescript-6-0-will-break-your-build-fix-it-first-7666eec2335a](https://thinkingthroughcode.medium.com/typescript-6-0-will-break-your-build-fix-it-first-7666eec2335a)  
14. TypeScript 6.0 Is Here, And It Changes More Than You Think | by Ripenapps Technologies, accessed July 4, 2026, [https://medium.com/@ripenapps-technologies/typescript-6-0-is-here-and-it-changes-more-than-you-think-91a241f96682](https://medium.com/@ripenapps-technologies/typescript-6-0-is-here-and-it-changes-more-than-you-think-91a241f96682)  
15. Documentation \- TypeScript 6.0, accessed July 4, 2026, [https://www.typescriptlang.org/docs/handbook/release-notes/typescript-6-0.html](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-6-0.html)  
16. How to Write an AGENTS.md File: The Complete Guide 2026 \- Atlan, accessed July 4, 2026, [https://atlan.com/know/how-to-write-agents-md/](https://atlan.com/know/how-to-write-agents-md/)  
17. Install TypeScript declarations for Bun, accessed July 4, 2026, [https://bun.com/docs/guides/runtime/typescript](https://bun.com/docs/guides/runtime/typescript)  
18. TypeScript 6 and 7 \- Bun, accessed July 4, 2026, [https://bun.com/docs/typescript-6](https://bun.com/docs/typescript-6)  
19. Announcing TypeScript 6.0 Beta \- Medium, accessed July 4, 2026, [https://medium.com/@onix\_react/announcing-typescript-6-0-beta-38fe5b94b02b](https://medium.com/@onix_react/announcing-typescript-6-0-beta-38fe5b94b02b)  
20. TypeScript 6.0 Released: The Final JavaScript-Based Version, accessed July 4, 2026, [https://socket.dev/blog/typescript-6-0-released-final-javascript-based-version](https://socket.dev/blog/typescript-6-0-released-final-javascript-based-version)  
21. The TSConfig Cheat Sheet \- Total TypeScript, accessed July 4, 2026, [https://www.totaltypescript.com/tsconfig-cheat-sheet](https://www.totaltypescript.com/tsconfig-cheat-sheet)  
22. Documentation \- TypeScript 5.8, accessed July 4, 2026, [https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-8.html](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-8.html)  
23. TSConfig Option: erasableSyntaxOnly \- TypeScript, accessed July 4, 2026, [https://www.typescriptlang.org/tsconfig/erasableSyntaxOnly.html](https://www.typescriptlang.org/tsconfig/erasableSyntaxOnly.html)  
24. TypeScript 5.8 Features Every Developer Should Know in 2026 \- DEV Community, accessed July 4, 2026, [https://dev.to/whoffagents/typescript-58-features-every-developer-should-know-in-2026-37oj](https://dev.to/whoffagents/typescript-58-features-every-developer-should-know-in-2026-37oj)  
25. Mastering the Latest TypeScript: What's New in 6.0 (and a Peek at 7\) \- DEV Community, accessed July 4, 2026, [https://dev.to/erikch/mastering-the-latest-typescript-whats-new-in-60-and-a-peek-at-7-4m8o](https://dev.to/erikch/mastering-the-latest-typescript-whats-new-in-60-and-a-peek-at-7-4m8o)  
26. Sensible tsconfig.json Defaults \- Patrick Kerschbaum, accessed July 4, 2026, [https://patricktree.me/tidbits/sensible-tsconfig-defaults](https://patricktree.me/tidbits/sensible-tsconfig-defaults)  
27. Documentation \- TypeScript 5.2, accessed July 4, 2026, [https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-2.html](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-2.html)  
28. What's New in TypeScript 6.0 \- OpenReplay Blog, accessed July 4, 2026, [https://blog.openreplay.com/whats-new-typescript-6-0/](https://blog.openreplay.com/whats-new-typescript-6-0/)  
29. TypeScript using Keyword and Explicit Resource Management: Done Right | jsmanifest, accessed July 4, 2026, [https://jsmanifest.com/typescript-using-explicit-resource-management](https://jsmanifest.com/typescript-using-explicit-resource-management)  
30. JavaScript resource management \- MDN Web Docs, accessed July 4, 2026, [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Resource\_management](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Resource_management)  
31. Add support for the "using" keyword and resource management · oven-sh bun · Discussion \#4325 \- GitHub, accessed July 4, 2026, [https://github.com/oven-sh/bun/discussions/4325](https://github.com/oven-sh/bun/discussions/4325)  
32. Notes on TypeScript 5.6, accessed July 4, 2026, [https://effectivetypescript.com/2024/09/30/ts-56/](https://effectivetypescript.com/2024/09/30/ts-56/)  
33. Bun Runtime, accessed July 4, 2026, [https://bun.com/docs/runtime](https://bun.com/docs/runtime)  
34. The Fastest Way to Build the Best TypeScript Development Environment \- Autumn 2025, accessed July 4, 2026, [https://zenn.dev/somnicattus/articles/3c1f3756aec24a?locale=en](https://zenn.dev/somnicattus/articles/3c1f3756aec24a?locale=en)  
35. How to Write Tests with Bun Test Runner \- OneUptime, accessed July 4, 2026, [https://oneuptime.com/blog/post/2026-01-31-bun-testing/view](https://oneuptime.com/blog/post/2026-01-31-bun-testing/view)  
36. React app consuming internal packages in a Bun workspace. Patterns that worked, and questions. : r/reactjs \- Reddit, accessed July 4, 2026, [https://www.reddit.com/r/reactjs/comments/1oi3ep1/react\_app\_consuming\_internal\_packages\_in\_a\_bun/](https://www.reddit.com/r/reactjs/comments/1oi3ep1/react_app_consuming_internal_packages_in_a_bun/)  
37. Managing TypeScript Packages in Monorepos | Nx Blog, accessed July 4, 2026, [https://nx.dev/blog/managing-ts-packages-in-monorepos](https://nx.dev/blog/managing-ts-packages-in-monorepos)  
38. TSConfig Option: noUncheckedSideEffectImports \- TypeScript, accessed July 4, 2026, [https://www.typescriptlang.org/tsconfig/noUncheckedSideEffectImports.html](https://www.typescriptlang.org/tsconfig/noUncheckedSideEffectImports.html)  
39. TypeScript 6.0 and CSS Side-Effect Imports: What Changed and How to Fix It, accessed July 4, 2026, [https://schalkneethling.com/posts/typescript-6-0-and-css-side-effect-imports-what-changed-and-how-to-fix-it/](https://schalkneethling.com/posts/typescript-6-0-and-css-side-effect-imports-what-changed-and-how-to-fix-it/)  
40. Biome, toolchain of the web, accessed July 4, 2026, [https://biomejs.dev/](https://biomejs.dev/)  
41. Biome.js: Configs for JS/TS, React & Angular (Plus Ignoring the Noise) \- NashTech Blog, accessed July 4, 2026, [https://blog.nashtechglobal.com/biome-js-configs-for-js-ts-react-angular-plus-ignoring-the-noise/](https://blog.nashtechglobal.com/biome-js-configs-for-js-ts-react-angular-plus-ignoring-the-noise/)  
42. styleguide.md \- basarat/typescript-book \- GitHub, accessed July 4, 2026, [https://github.com/basarat/typescript-book/blob/master/docs/styleguide/styleguide.md](https://github.com/basarat/typescript-book/blob/master/docs/styleguide/styleguide.md)  
43. Linter | Biome, accessed July 4, 2026, [https://biomejs.dev/linter/](https://biomejs.dev/linter/)  
44. useNamingConvention \- Biome.js, accessed July 4, 2026, [https://biomejs.dev/linter/rules/use-naming-convention/](https://biomejs.dev/linter/rules/use-naming-convention/)  
45. Linter rules from other sources · biomejs biome · Discussion \#3 \- GitHub, accessed July 4, 2026, [https://github.com/biomejs/biome/discussions/3](https://github.com/biomejs/biome/discussions/3)  
46. Rules sources \- Biome.js, accessed July 4, 2026, [https://biomejs.dev/linter/rules-sources/](https://biomejs.dev/linter/rules-sources/)  
47. JavaScript Rules \- Biome.js, accessed July 4, 2026, [https://biomejs.dev/linter/javascript/rules/](https://biomejs.dev/linter/javascript/rules/)  
48. Setup a React Vite project with TypeScript, Prettier & Vitest \[2024\] | by Sasha Nedopaka, accessed July 4, 2026, [https://medium.com/@nedopaka/setup-a-react-vite-project-with-typescript-prettier-vitest-2024-9bb6e919ac8f](https://medium.com/@nedopaka/setup-a-react-vite-project-with-typescript-prettier-vitest-2024-9bb6e919ac8f)  
49. Bun \- Glossary \- MDN Web Docs, accessed July 4, 2026, [https://developer.mozilla.org/en-US/docs/Glossary/Bun](https://developer.mozilla.org/en-US/docs/Glossary/Bun)  
50. Mocks \- Bun, accessed July 4, 2026, [https://bun.com/docs/test/mocks](https://bun.com/docs/test/mocks)  
51. TypeScript v6 is here: A full migration guide \- LogRocket Blog, accessed July 4, 2026, [https://blog.logrocket.com/typescript-v6-migration-guide/](https://blog.logrocket.com/typescript-v6-migration-guide/)  
52. TypeScript Biome vs ESLint: Linting and Formatting Comparison 2026 \- Reintech, accessed July 4, 2026, [https://reintech.io/blog/typescript-biome-vs-eslint-linting-formatting-comparison-2026](https://reintech.io/blog/typescript-biome-vs-eslint-linting-formatting-comparison-2026)  
53. Biome vs ESLint (2026) \- Which Infrastructure Tool? \- Developers Digest, accessed July 4, 2026, [https://www.developersdigest.tech/compare/biome-vs-eslint](https://www.developersdigest.tech/compare/biome-vs-eslint)  
54. ESLint vs Biome: JavaScript Linting Comparison 2026 | Complete Guide \- Reintech, accessed July 4, 2026, [https://reintech.io/blog/eslint-vs-biome-javascript-linting-comparison-2026](https://reintech.io/blog/eslint-vs-biome-javascript-linting-comparison-2026)  
55. Biome vs ESLint vs Oxlint 2026: Which JS Linter to Pick \- PkgPulse, accessed July 4, 2026, [https://www.pkgpulse.com/guides/biome-vs-eslint-vs-oxlint-2026](https://www.pkgpulse.com/guides/biome-vs-eslint-vs-oxlint-2026)  
56. Follow TypeScript best practices \- AWS Prescriptive Guidance, accessed July 4, 2026, [https://docs.aws.amazon.com/prescriptive-guidance/latest/best-practices-cdk-typescript-iac/typescript-best-practices.html](https://docs.aws.amazon.com/prescriptive-guidance/latest/best-practices-cdk-typescript-iac/typescript-best-practices.html)  
57. Spy on methods in \`bun test\`, accessed July 4, 2026, [https://bun.com/docs/guides/test/spy-on](https://bun.com/docs/guides/test/spy-on)  
58. bun:test module | API Reference, accessed July 4, 2026, [https://bun.com/reference/bun/test](https://bun.com/reference/bun/test)