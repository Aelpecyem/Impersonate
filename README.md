# Impersonate

[![Curseforge](https://curse.nikky.moe/api/img/360333?logo)](https://www.curseforge.com/projects/360333) [![](https://jitpack.io/v/Ladysnake/Impersonate.svg)](https://jitpack.io/#Ladysnake/Impersonate)

A library handling player impersonation. Can also be used as a standalone mod, through the `/impersonate` command.

## Overview

Impersonate allows players to take on the name and appearance of other players. When impersonating someone, a player
will:
- have the skin of the impersonated player  
- have the nameplate of the impersonated player  
- send every chat message as if they were the impersonated player  
- appear as the impersonated players in online player lists  
- sign books as if they were the impersonated player  
- etc.  

They will however not fool the impersonated player's pets, if any.

To avoid moderation chaos, server logs will always display the actual player's name, alongside their fake identity.

### Commands  
Impersonate adds the `/impersonate` command, allowing server operators and mapmakers to interact with the API through
commands.  
- `/impersonate disguise`  
    - `/impersonate disguise as <name> [<targets>] [<key>]` : disguises one or more players  
        - `<name>` : Name of the player to impersonate. Does not have to be online or even real.  
        - `[<targets>]` (optional) : If specified, must be either a player's username or a target selector. If unspecified, defaults to the player using the command. When used in a command block, player is not optional.  
        - `[<key>]` (optional) : If specified, must be a valid identifier serving as a key for the impersonation.  
    - `/impersonate disguise clear [<targets>] [<key>]` : stops the impersonation of one or more players  
        - `[<targets>]` (optional) : If specified, must be either a player's username or a target selector. If unspecified, defaults to the player using the command. When used in a command block, player is not optional.  
        - `[<key>]` (optional) : If specified, must be a valid identifier that was previously used as a key to start an impersonation. If left unspecified, the command will clear every active impersonation.  
    
### Gamerules
- `impersonate:fakeCapes` : Whether impersonators should get the cape and elytra of impersonated players. Defaults to `false`.  
- `impersonate:opRevealImpersonations` : Whether impersonations should be revealed to online server operators like they get revealed in the server logs. Defaults to `true`.  

## Adding Impersonate to your project

You can add the library by inserting the following in your `build.gradle` :

```gradle
repositories {
	maven { 
        name = "Ladysnake Libs"
        url = 'https://dl.bintray.com/ladysnake/libs' }
    }
    maven {
        name = "NerdHubMC"
        url = "https://maven.abusedmaster.xyz"
    }
}

dependencies {
    modImplementation "io.github.ladysnake:Impersonate:${impersonate_version}"
    include "io.github.ladysnake:Impersonate:${impersonate_version}"
    // Impersonate dependencies
    include "com.github.NerdHubMC.Cardinal-Components-API:cardinal-components-base:${cca_version}"
    include "com.github.NerdHubMC.Cardinal-Components-API:cardinal-components-entity:${cca_version}"
}
```

You can then add the library version to your `gradle.properties`file:

```properties
# Impersonate
impersonate_version = 1.x.y
```

You can find the current version of Impersonate in the [releases](https://github.com/Ladysnake/Impersonate/releases) tab of the repository on Github.

## Using Impersonate

You can find examples in the [Test Mod](https://github.com/Ladysnake/Impersonate/tree/master/src/testmod/java/io/github/ladysnake/impersonatest)
and in the [Impersonate Command](https://github.com/Ladysnake/Impersonate/blob/master/src/main/java/io/github/ladysnake/impersonate/impl/ImpersonateCommand.java).

```java
public static final Identifier IMPERSONATION_KEY = new Identifier("mymod", "impersonitem");

public boolean useOnEntity(ItemStack stack, PlayerEntity user, LivingEntity entity, Hand hand) {
    if (entity instanceof ServerPlayerEntity) {
        Impersonator.get(user).impersonate(IMPERSONATION_KEY, ((PlayerEntity) entity).getGameProfile());
    } else {
        Impersonator.get(user).stopImpersonation(IMPERSONATION_KEY);
    }
    return super.useOnEntity(stack, user, entity, hand);
}
```
