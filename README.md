# DigMem

DigMem is a self-evolving memory system designed for agents.

Core features include:

- Append-only history tracking
- Tree-based memory digestion and evolvement
- Version control system compatible

Check [philosophy](doc/writing/philosophy.md) and [architecture](doc/writing/architecture/general.md) for details behind them.

## Quick start

``` bash
npm install -g digmem

cd your-project/
digmem init

# Instruct your agents about DigMem usage
echo "Read .digmem/AGENTS.md for digmem tool usage" >> AGENTS.md
```
