# Capability - Code Analysis

> Tags: Analysis, Code Style, Code Rules, Roslyn

The Lithium Framework enforces a development environment in which static and live code analysis is always present and automatically configured in all projects to enforce and ensure good coding practices.

This is provided by [Hydrogen Design-time](../ref/hydrogen-designtime-2.0/README.md) with a set of Roslyn Analyzers and a standard definition for the rules applied by these analyzers.

Code analysis is applied both live, while the programming is writing his code, and at build time (on the development machine and on the build servers).