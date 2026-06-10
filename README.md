# Swiss Bank Transactions Sync - Replication Router

A React/Vite Distributed Systems demo that visualizes master-to-replica transactional replication behavior for a banking ledger. The app demonstrates strict write durability, safe read load-balancing, replication-lag fallback, and stale replica isolation.

## What Was Implemented

- Visual replication-lag slider for Master-to-Replica transactional latency.
- Live replica states: Fresh, At Risk, Blocked over 300ms, and Isolated at 450ms.
- SELECT read routing across healthy regional replicas.
- Automatic SELECT fallback to the active master when replica lag crosses 300ms.
- Strict INSERT routing to the active master node `postgresql://main-master`.
- Regional partition decay simulation that degrades replica links.
- Stale replica isolation at the 450ms boundary to prevent reading outdated rows.
- Policy callouts explaining active fallback, isolation, and strict write routing.
- Live throughput metrics for writes, reads, eligible replicas, and maximum lag.
- Criteria Coverage tab mapping each assignment requirement to a UI proof step.

## Project Structure

```text
index.html          Vite HTML entry point
src/main.jsx        React application and routing/replication logic
src/styles.css      Application styling
package.json        Scripts and dependencies
package-lock.json   Locked dependency versions
```

## Environment Requirements

Install Node.js first if it is not already installed.

Recommended:

- Node.js 18 or newer
- npm, included with Node.js

Check versions:

```powershell
node --version
npm.cmd --version
```

## Run Locally

After cloning the repository, open PowerShell inside the project folder and run:

```powershell
npm install
```

Start the development server:

```powershell
npm.cmd run dev
```

Open the URL printed by Vite. By default it is usually:

```text
http://127.0.0.1:5173
```

If port 5173 is busy, Vite may use another port. Use the URL shown in the terminal.

## Build Test

To verify the project compiles:

```powershell
npm.cmd run build
```

To preview the production build after building:

```powershell
npm.cmd run preview
```

## How To Demonstrate The Assignment Requirements

### 1. Visual Replication Lag Slider

1. Open the `Live Sandbox` tab.
2. Move the `Master-to-Replica transactional latency` slider.
3. Watch each replica card update its lag, link percentage, state label, route bar, and max-lag metric.

### 2. SELECT Fallback Over 300ms

1. Set the latency slider above 300ms.
2. Click `SELECT statement read`.
3. Confirm `Route Decision` changes to `postgresql://main-master`.
4. Check the Policy Callouts section for `Read fallback engaged`.

### 3. Clear Fallback Callouts

1. Raise latency over 300ms or enable `Regional partition decay`.
2. Read the visible Policy Callouts.
3. The UI explains why reads are rerouted or replicas are isolated.

### 4. Strict Durability Split For INSERT vs SELECT

1. Click `INSERT wire transfer validation`.
2. Confirm it always routes to `postgresql://main-master`.
3. Lower latency under 300ms.
4. Click `SELECT statement read`.
5. Confirm healthy reads are load-balanced across eligible replica DSNs.

### 5. Regional Partition Decay And 450ms Isolation

1. Enable `Regional partition decay`, or raise latency close to 450ms.
2. Watch replica cards change to `Isolated 450ms` when stale.
3. Confirm isolated replicas move to `Removed from read pool`.
4. Confirm SELECT traffic no longer uses isolated replicas.

## Notes For The Instructor

This project is not committed with `node_modules`. Please run `npm install` after cloning, then `npm.cmd run dev` to start the Vite development server.
