# Sprbrk.Poc.DesignLibrary

This repository is a **proof-of-concept (POC)** demonstrating how a shared component library can be integrated into another repository using **Git Subtree**.

The goal is to simulate how a reusable UI component library (similar to the Springbrook Design Library) can live in its own repository while still being embedded directly into a consuming application repository.

---

# Repositories in this POC

Two repositories participate in this demonstration.

## 1. Design Library

`Sprbrk.Poc.DesignLibrary`

This repository contains a **Razor Class Library** with reusable Blazor components.

Example component:

`Component1.razor`

This component is used by the consuming application.

---

## 2. Consuming Application

`Cirrus.BlazorServer`

This repository contains a **Blazor Server application** that consumes the design library.

The design library is embedded inside this repository using **Git Subtree**.

---

# Architecture

After the subtree integration, the structure of the consuming repository looks like this:

```
Cirrus.BlazorServer
│
├ Cirrus.BlazorServer
│   └ Blazor Server application
│
└ src
   └ design-library
      └ Sprbrk.Poc.DesignLibrary
```

The embedded design library behaves like a normal project in the solution and is referenced using a standard `.csproj` ProjectReference.

This allows components in the design library to be used directly by the application.

---

# What This POC Demonstrates

This project demonstrates the complete **Git Subtree workflow**.

## 1. Embedding a repository

The design library repository is embedded into the consuming repository.

```
git subtree add --prefix=src/design-library designlib main --squash
```

---

## 2. Pulling updates from the design library

If the design library repository changes, those updates can be pulled into the consuming repository.

```
git subtree pull --prefix=src/design-library designlib main --squash
```

This updates the embedded library while keeping both repositories independent.

---

## 3. Pushing fixes back upstream

If a change is made to the design library from within the consuming repository, it can be pushed back to the original repository.

```
git subtree push --prefix=src/design-library designlib main
```

This allows developers working in the consuming application to contribute improvements back to the shared library.

---

# Example Workflow

## Step 1 – Update the Design Library

Modify a component in the design library repository.

Example:

`Component1.razor`

Commit and push the change.

---

## Step 2 – Pull the update into the consuming application

Inside the consuming repository run:

```
git subtree pull --prefix=src/design-library designlib main --squash
```

The application now receives the updated component.

---

## Step 3 – Make a change from the consuming application

Modify the embedded library inside:

```
src/design-library/
```

Commit the change in the consuming repository.

---

## Step 4 – Push the change back to the design library repository

```
git subtree push --prefix=src/design-library designlib main
```

The update is now reflected in the original design library repository.

---

# Why Git Subtree?

Git Subtree allows teams to:

• Maintain a **shared library in its own repository**  
• Embed that library **directly into a consuming project**  
• Keep the **full history of both repositories**  
• Allow **bidirectional updates** between repositories

Compared to alternatives:

| Approach | Tradeoffs |
|--------|--------|
| NuGet Package | Requires publishing and version management |
| Git Submodule | Requires additional clone/update steps |
| Git Subtree | Embedded repo with simpler developer workflow |

---

# Purpose of This Repository

This repository exists solely to demonstrate and validate a possible workflow for integrating a shared component library into another repository using Git Subtree.

It is intended to help evaluate whether this approach could be suitable for managing shared UI components across multiple projects.