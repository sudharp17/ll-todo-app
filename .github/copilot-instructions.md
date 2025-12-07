# GitHub Copilot Instructions — ll-todo-app

This document provides comprehensive guidance for using GitHub Copilot effectively with this Next.js + Bun todo application and establishes clear code review standards.

## 🏗️ Project Overview

**Tech Stack:**
- **Framework:** Next.js `^15.0.0`
- **Runtime:** Bun `1.3+` (dev server + testing)
- **UI:** React `^19.0.0-rc` (pre-release)
- **State Management:** Zustand `^4.4.7`
- **Styling:** Tailwind CSS + PostCSS
- **Language:** TypeScript

**Architecture Pattern:** Clean separation between UI components, business logic, and state management.

## 📁 Project Structure
```
ll-todo-app/
├── app/
│   ├── layout.tsx           # Root layout
│   ├── page.tsx            # Home page
│   ├── globals.css         # Global styles
│   ├── components/         # UI components
│   │   ├── AddTodoForm.tsx
│   │   ├── TodoItem.tsx
│   │   └── TodoList.tsx
│   └── lib/
│       ├── logic/
│       │   └── todoLogic.ts    # Pure business logic
│       ├── store/
│       │   ├── StoreProvider.tsx
│       │   └── todoStore.ts    # Zustand store
│       └── types/
│           └── todo.ts         # TypeScript interfaces
├── tests/
│   ├── logic/
│   │   └── todoLogic.test.ts
│   └── store/
│       └── todoStore.test.ts
└── package.json
```

## 🎯 Development Commands
```bash
# Install dependencies
bun install

# Development server
bun --bun run next dev

# Run tests
bun test

# Build for production
bun run build

# Start production server
bun run start

# Lint code
bun run lint
```

## 🎨 Coding Conventions

### TypeScript
- **Strict typing:** No `any` types; use explicit interfaces from `lib/types/todo.ts`
- **Pure functions:** All business logic in `lib/logic/` should be pure and testable
- **Immutability:** Never mutate function parameters; return new objects/arrays

### React Components
- **Server-first:** Prefer server components; use `"use client"` only when necessary
- **Minimal props:** Components receive only what they need
- **Stable keys:** Use unique, stable IDs for list rendering (never array indices)

### State Management
- **Centralized:** All state operations go through `todoStore.ts`
- **Thin actions:** Store actions delegate to pure functions in `todoLogic.ts`
- **Selective subscriptions:** Use Zustand selectors to prevent unnecessary re-renders

### Styling
- **Utility-first:** Use Tailwind classes; avoid custom CSS unless necessary
- **Consistent spacing:** Follow established Tailwind patterns
- **Responsive design:** Mobile-first approach

## 🤖 Copilot Prompt Templates

### For Business Logic
```
"Implement addTodo, toggleTodo, and removeTodo functions in lib/logic/todoLogic.ts. 
Make them pure functions that respect the Todo interface from lib/types/todo.ts. 
Include input validation and return Result types for error handling."
```

### For React Components
```
"Create a controlled AddTodoForm component in app/components/AddTodoForm.tsx that:
- Uses todoStore.addTodo action on form submission
- Validates input (no empty titles)
- Resets form after successful submission
- Includes proper TypeScript types and accessibility attributes"
```

### For State Management
```
"Update todoStore.ts to use selectors for performance optimization. 
Create separate selectors for todos, completed todos, and pending todos.
Ensure actions remain thin wrappers around logic functions."
```

### For Testing
```
"Write comprehensive Bun tests for lib/logic/todoLogic.ts covering:
- Happy path scenarios (add, toggle, remove)
- Edge cases (empty strings, duplicate IDs)
- Input validation
- Ensure all functions remain pure"
```

## ✅ Code Review Checklist

### Architecture & Design
- [ ] **Separation of concerns:** UI components don't contain business logic
- [ ] **Pure functions:** Logic functions are testable and don't cause side effects
- [ ] **Type safety:** All functions and components have explicit TypeScript types
- [ ] **Server/Client boundaries:** `"use client"` used only when hooks/state required

### Performance & Optimization
- [ ] **Rendering efficiency:** List items use stable keys, avoid expensive computations in render
- [ ] **State subscriptions:** Components subscribe to minimal state via selectors
- [ ] **Unnecessary re-renders:** No prop drilling; proper memoization where needed
- [ ] **Bundle size:** No unused dependencies or excessive imports

### Accessibility (a11y)
- [ ] **Semantic HTML:** Proper use of buttons, inputs, labels, and ARIA attributes
- [ ] **Keyboard navigation:** All interactive elements accessible via keyboard
- [ ] **Focus management:** Logical focus flow, especially after form submissions
- [ ] **Screen reader support:** Meaningful alt texts and ARIA labels

### Testing & Quality
- [ ] **Unit test coverage:** New logic functions have corresponding tests
- [ ] **Test reliability:** Tests are deterministic and don't depend on external state
- [ ] **Edge case handling:** Tests include error conditions and boundary cases
- [ ] **Type checking:** TypeScript compilation passes without errors

### Security & Robustness
- [ ] **Input validation:** All user inputs are validated and sanitized
- [ ] **XSS prevention:** No dangerouslySetInnerHTML or unsafe dynamic content
- [ ] **Dependency security:** No known vulnerabilities in package dependencies
- [ ] **Error boundaries:** Proper error handling and user feedback

### Code Style & Maintainability
- [ ] **Consistent formatting:** Code follows project ESLint rules
- [ ] **Clear naming:** Functions, variables, and components have descriptive names
- [ ] **Documentation:** Complex logic has clear comments explaining the "why"
- [ ] **File organization:** New files follow established project structure

## 🔍 Review Prompts for Copilot

### Performance Review
```
"Analyze TodoList.tsx for performance issues. Check for unnecessary re-renders, 
inefficient selectors, or missing React.memo usage. Suggest specific optimizations 
with code examples."
```

### Accessibility Audit
```
"Review AddTodoForm.tsx for accessibility compliance. Check ARIA labels, 
keyboard navigation, focus management, and screen reader support. 
Provide specific fixes for any issues found."
```

### Logic Validation
```
"Examine lib/logic/todoLogic.ts for edge cases and potential bugs. 
Check input validation, error handling, and function purity. 
Suggest additional test cases for uncovered scenarios."
```

### Architecture Assessment
```
"Evaluate the separation of concerns in TodoItem.tsx. Ensure UI logic 
is separate from business logic, props are minimal, and the component 
follows React best practices. Suggest refactoring if needed."
```

## 🚀 PR Template

When submitting pull requests, include:

**Summary**
- What: Brief description of changes
- Why: Problem being solved or feature being added

**Technical Details**
- Files modified and reason for each change
- Architecture decisions and trade-offs considered
- Dependencies added or removed

**Testing**
- [ ] Tests added/updated for new functionality
- [ ] All existing tests pass: `bun test`
- [ ] Linting passes: `bun run lint`
- [ ] Build succeeds: `bun run build`

**Accessibility**
- [ ] Keyboard navigation tested
- [ ] Screen reader compatibility verified
- [ ] ARIA labels added where appropriate

**Performance**
- [ ] No unnecessary re-renders introduced
- [ ] Bundle size impact considered
- [ ] Loading performance maintained

## 🔧 Troubleshooting

### React 19 RC Considerations
- Pre-release APIs may change; prefer stable patterns
- If compatibility issues arise, consider pinning React 18 temporarily
- Monitor React 19 release notes for breaking changes

### Bun-Specific Notes
- Use `bun --bun` flag for Next.js dev server to use Bun runtime
- Bun's test runner has different syntax than Jest; refer to Bun docs
- Hot reload may behave differently than Node.js-based tools

### Common Issues
- **Module resolution:** Ensure import paths use absolute imports from project root
- **Type conflicts:** Check for version mismatches between @types packages
- **Build failures:** Verify all dependencies are compatible with Next.js 15

## 📚 Quick Reference

**Key Files to Reference:**
- `lib/types/todo.ts` - Type definitions
- `lib/logic/todoLogic.ts` - Business logic
- `lib/store/todoStore.ts` - State management
- `copilot-instructions.md` - This file

**Useful Copilot Commands:**
- `/explain` - Understand existing code
- `/fix` - Suggest fixes for problems
- `/tests` - Generate test cases
- `/doc` - Add documentation

Keep prompts specific, reference exact files, and align with these conventions for consistent, high-quality code generation and reviews.