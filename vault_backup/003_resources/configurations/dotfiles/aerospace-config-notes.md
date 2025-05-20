
### Monitor delecration

**home/itf set up**
```sh
# Force certain workspaces to appear on specific monitors
# Monitor names can be found using the `monitors` command in Aerospace or guessed based on order
[workspace-to-monitor-force-assignment]
1 = 1
2 = 1
3 = 1
4 = 1
5 = ['3', '2']
6 = ['3', '2']
7 = ['3', '2']
8 = ['3', '2']
9 = 2
B = 1
C = 2
D = ['3', '2']
E = ['3', '2']
F = 2
M = 2
N = ['3', '2']
R = 1
T = 1
W = 1
```

**lc setup**
```sh
[workspace-to-monitor-force-assignment]
1 = 2
2 = 2
3 = 2
4 = 2
5 = ['3', '1']
6 = ['3', '1']
7 = ['3', '1']
8 = ['3', '1']
9 = 1
B = 2
C = 1
D = ['3', '1']
E = ['3', '1']
F = 1
M = 1
N = ['3', '1']
R = 2
T = 2
W = 2
```