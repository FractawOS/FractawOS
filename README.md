# FractawOS

FractawOS is a modular open-source system environment built on top of Windows.

It provides a custom system layer, interface, services, automation model, and module ecosystem while preserving native Windows compatibility.

FractawOS is not a replacement kernel.

Windows remains responsible for the underlying kernel, drivers, hardware compatibility, security infrastructure, and native application execution.

FractawOS provides the primary user experience and system orchestration layer above it.

## Architecture

The platform is composed of a base system and optional modules.

The base system remains functional without optional modules.

```text
Windows
   ↓
FractawOS
├── Shell
├── Core
├── App Manager
├── Group Manager
├── Module Manager
├── Resource Manager
├── Telemetry
├── Audio Fabric
├── Device Manager
├── Settings
├── Notifications
├── Update Manager
├── Recovery
└── Windows Integration
       ↓
Modules
├── FractawDevModule
├── FractawGameModule
├── FractawAudioModule
└── Community Modules
```

## Modules

Modules provide specialized capabilities.

The first official modules are:

- FractawDevModule
- FractawGameModule
- FractawAudioModule

Modules are separate projects.

They interact with FractawOS through contracts defined by FractawModuleSDK.

The Core should not contain domain-specific knowledge that belongs to modules.

## Groups

Applications are not permanently assigned to modules.

Users organize applications into groups.

A group may define:

- applications;
- required modules;
- module policy overrides;
- Core policy overrides.

Example:

```text
Development Group

Applications
├── Visual Studio Code
├── Windows Terminal
└── DBeaver

Modules
└── FractawDevModule

Policies
├── Start WSL
├── Start PostgreSQL
└── Development resource profile
```

Opening an application belonging to the group may activate the required modules automatically.

## Native group

FractawOS provides a default Native group.

Applications inside the Native group do not require an optional module.

Examples may include browsers, Spotify, Discord, file managers, and other general-purpose applications.

New applications may initially be assigned to the Native group until the user chooses another configuration.

## Policy inheritance

Modules define the policies they support and their default values.

Groups inherit those policies.

Users may override them per group.

Conceptually:

```text
Core defaults
      ↓
Module defaults
      ↓
Group overrides
```

Future versions may also support application-level overrides.

## Module lifecycle

Modules are managed by the FractawOS runtime.

A module may move between states such as:

```text
Stopped
Starting
Active
Idle
Suspended
Degraded
```

The user should not normally need to manually start modules.

Opening an application or capability may activate the required module automatically.

## Community modules

FractawOS is designed as an extensible platform.

Third-party developers may create modules using FractawModuleSDK.

Modules may live outside the FractawOS GitHub organization.

Compatible modules may be submitted to FractawModuleRegistry.

The registry may classify modules as:

- Community
- Verified
- Official

## Design principles

FractawOS should remain:

- modular;
- user-controlled;
- recoverable;
- observable;
- extensible;
- compatible with Windows;
- independent from individual modules;
- stable under partial failure.

A module failure should not unnecessarily compromise unrelated platform capabilities.

## Windows compatibility model

FractawOS is intended to run as an independent software layer on a properly licensed Windows installation.

The project does not distribute Windows, replace the Windows kernel, or depend on redistributing modified Microsoft system components.

Where possible, FractawOS should use documented and supported Windows interfaces and preserve a recovery path to the standard Windows environment.

FractawOS is an independent open-source project and is not affiliated with, endorsed by, or sponsored by Microsoft Corporation.

Windows is a trademark of the Microsoft group of companies.

## Status

FractawOS is currently in the architecture and early development stage.

Public interfaces, module contracts, and implementation details may change while the platform is being established.
