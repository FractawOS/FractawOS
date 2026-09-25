# FractawOS Architecture

## Overview

FractawOS is a modular platform layer built on top of Windows.

Windows remains the host operating system.

FractawOS provides the primary shell, orchestration services, resource policies, telemetry, module runtime, and extensibility model.

The architecture separates platform responsibilities from domain-specific functionality.

## Platform boundary

The FractawOS Core is responsible for generic system capabilities.

Optional modules are responsible for specialized domains.

The Core must not require FractawDevModule, FractawGameModule, FractawAudioModule, or any third-party module to function.

## Base platform

The base platform includes:

- Shell;
- Core services;
- application management;
- group management;
- module runtime;
- resource management;
- telemetry;
- Audio Fabric;
- device management;
- settings;
- notifications;
- update infrastructure;
- recovery mechanisms;
- Windows integration.

## Module model

Modules extend the platform through public interfaces.

Modules may declare:

- capabilities;
- policies;
- permissions;
- lifecycle hooks;
- events;
- settings;
- optional user interface extensions.

Modules communicate with the Core through public contracts defined by FractawModuleSDK.

Modules should not directly depend on internal Core implementation details.

## Groups

Groups are user-defined contexts.

They connect applications to modules and policies.

Applications do not inherently belong to a module.

The user controls the association.

A group may activate zero, one, or multiple modules.

Groups may also exist without any module dependency.

The Native group is the default context for applications that do not require optional modules.

## Policy model

Policies originate from the component responsible for the behavior.

Core policies are defined by the Core.

Module-specific policies are defined by modules.

Groups inherit defaults and may override them.

This avoids placing module-specific configuration inside the Core.

Conceptually:

```text
Core defaults
      ↓
Module defaults
      ↓
Group overrides
```

## Module activation

The system observes application and group state.

When an application associated with a group becomes active, the Group Manager resolves the group requirements.

Required modules are passed to the Module Manager.

The Module Manager ensures the modules are available and running.

Policies are then applied through the appropriate platform services.

Conceptually:

```text
Application
    ↓
App Manager
    ↓
Group Manager
    ↓
Policy Resolution
    ↓
Module Manager
    ↓
Resource / Device / Audio / Platform Services
```

## Module lifecycle

The runtime should support explicit module states so that activation and failure are observable and recoverable.

Initial lifecycle states include:

```text
Stopped
Starting
Active
Idle
Suspended
Degraded
```

Normal usage should not require the user to manually start modules.

Groups, capabilities, and user actions may activate them automatically.

## Failure isolation

Modules should fail independently whenever possible.

Examples:

- if FractawDevModule fails, general applications should continue running;
- if FractawAudioModule fails, general system audio should remain available through the base Audio Fabric whenever possible;
- if FractawGameModule fails, games should remain launchable as normal Windows applications.

The platform should prefer degraded functionality over complete system failure.

## Extensibility

FractawModuleSDK defines the supported extension boundary.

Community modules should be able to integrate without modifying the FractawOS source code.

If a module requires access to internal Core implementation details, this may indicate that the public SDK is missing an abstraction.

Modules may be maintained outside the FractawOS GitHub organization and later submitted to FractawModuleRegistry.

## Security

Modules must operate under an explicit permission model.

Sensitive capabilities should be exposed through mediated platform APIs instead of unrestricted direct access whenever practical.

Permission requests should be visible to the user.

Modules should request the minimum access necessary.

## Windows compatibility

FractawOS is designed to run as an independent software layer on a properly licensed Windows installation.

The project should avoid invasive modifications to Windows internals and should prefer documented and supported Windows mechanisms where practical.

This is particularly important for:

- driver compatibility;
- system updates;
- gaming anti-cheat systems;
- hardware support;
- security boundaries;
- recovery.

A standard Windows environment should remain available as a recovery or compatibility path where the edition and deployment model allow it.

FractawOS does not replace the Windows kernel and should not depend on redistributing modified Microsoft system components.

## Long-term goal

FractawOS should become a platform where users can build their own system experience by combining:

- applications;
- groups;
- modules;
- policies;
- community extensions;

without giving up the compatibility of the underlying Windows environment.
