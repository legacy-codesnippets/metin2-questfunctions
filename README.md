# Metin2 Questfunktionen (40k Source) – Erweiterungen von Avenue

Diese Sammlung umfasst zusätzliche Lua-Questfunktionen für `pc` und `npc` zur Verwendung im Metin2 Server (40k Source).

📅 Veröffentlichung: [29.03.2014, 23:09](https://www.elitepvpers.com/forum/metin2-pserver-guides-strategies/3189606-release-questfunctions-40k-source-c.html)

---

## 🔧 Serverseitig – Datei: `questlua_pc.cpp`

### Neue Funktionen:

```cpp
int pc_get_ip(lua_State* L)
{
    LPCHARACTER ch = CQuestManager::instance().GetCurrentCharacterPtr();
    lua_pushstring(L, ch->GetDesc()->GetHostName());
    return 1;
}

int pc_kill(lua_State* L)
{
    LPCHARACTER ch = CQuestManager::instance().GetCurrentCharacterPtr();
    ch->Dead();
    return 0;
}

int pc_set_coins(lua_State * L)
{
    if (!lua_isnumber(L, 1)) {
        sys_err("invalid argument");
        return 0;
    }

    LPCHARACTER ch = CQuestManager::instance().GetCurrentCharacterPtr();
    long val = (long)lua_tonumber(L, 1);
    SQLMsg *msg = DBManager::instance().DirectQuery("UPDATE account.account SET coins = coins + '%ld' WHERE id = '%d'", val, ch->GetAID());

    if (msg->uiSQLErrno != 0) {
        sys_err("pc_update_coins query failed");
        return 0;
    }
    delete msg;
}

int pc_get_empire_name(lua_State* L)
{
    LPCHARACTER ch = CQuestManager::instance().GetCurrentCharacterPtr();
    const char* tabelle[3] = {"Shinsoo","Chunjo","Jinno"};
    int empireave = ch->GetEmpire() - 1;
    lua_pushstring(L, tabelle[empireave]);
    return 1;
}
```

### Registrierung:

```cpp
{ "get_ip",            pc_get_ip },
{ "kill",              pc_kill },
{ "set_coins",         pc_set_coins },
{ "get_empire_name",   pc_get_empire_name },
```

---

## 👤 NPC-Funktionen – Datei: `questlua_npc.cpp`

### Neue Funktionen:

```cpp
int npc_get_ip(lua_State* L)
{
    LPCHARACTER npc = CQuestManager::instance().GetCurrentNPCCharacterPtr();
    lua_pushstring(L, npc->GetDesc()->GetHostName());
    return 1;
}

int npc_get_level(lua_State* L)
{
    LPCHARACTER npc = CQuestManager::instance().GetCurrentNPCCharacterPtr();
    lua_pushnumber(L, npc->GetLevel());
    return 1;
}

int npc_get_name(lua_State* L)
{
    LPCHARACTER npc = CQuestManager::instance().GetCurrentNPCCharacterPtr();
    lua_pushstring(L, npc->GetName());
    return 1;
}

int npc_get_job(lua_State* L)
{
    LPCHARACTER npc = CQuestManager::instance().GetCurrentNPCCharacterPtr();
    lua_pushnumber(L, npc->GetJob());
    return 1;
}
```

### Registrierung:

```cpp
{ "get_ip",      npc_get_ip },
{ "get_level",   npc_get_level },
{ "get_name",    npc_get_name },
{ "get_job",     npc_get_job },
```

### Wichtiger Hinweis:

In `questlua_npc.cpp` ggf. folgendes am Anfang einfügen:

```cpp
#include "desc.h"
```

## 🔗 Quelle

- [elitepvpers Thread](https://www.elitepvpers.com/forum/metin2-pserver-guides-strategies/3189606-release-questfunctions-40k-source-c.html)
