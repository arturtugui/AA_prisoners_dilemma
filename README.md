# Forgiving Pavlov with History Hashing

This strategy is based on the **Pavlov strategy** but introduces a forgiveness mechanism and optimizes memory usage by considering only the last 30 moves of the history. It applies the following logic:

## Summary of Moves
1. **Cooperate first**.
2. **Repeat the last move** if both players did the same thing.
3. **Shift** if players did different things
4. **Forgive** the opponent with a 20% chance if the player cooperated and the opponent defected.

## 1. **First Move: Cooperate**
   - On the very first move the strategy cooperates (returns `1`).

## 2. **Repeating the Last Move**
   - If both players made the same move in the previous round (either both cooperated or both defected), the strategy repeats the last move.
   - If both players cooperated `(1, 1)` or both defected `(0, 0)`, the strategy continues with the same move.

## 3. **Forgiveness After Defection**
   - If the player cooperated (`1`) and the opponent defected (`0`), the strategy may defect (`0`), but it has a chance of forgiving the opponent and cooperating again.
   - Forgiveness happens with a **20% chance**. This chance is determined by hashing the last 30 moves of both the player and the opponent.
     - If the hash of the history modulo 5 equals `0`, the strategy forgives and cooperates again. Otherwise, it defects (punishes the opponent).
   - This forgiveness is applied only when the player cooperated and the opponent defected in the previous round.

## 4. **Shift Strategy After Opponent Cooperation**
   - If the player defected (`0`) in the previous round and the opponent cooperated (`1`), shift, i.e. (`1`).

## 5. **Efficient History Handling**
   - To prevent excessive memory usage, the strategy hashes only the **last 30 moves** from both the player and the opponent, ensuring that the history is manageable and avoids using too much memory.
   - The hash is used to determine the forgiveness chance without relying on the full history, making the algorithm more efficient.
