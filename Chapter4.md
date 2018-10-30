# Chapter 4
## Carry Look Ahead Adder
* Generate and propagate functions
  * `G_i = x_i * y_i`
  * `P_i = x_i + y_1`
* For any `C_(i+1) = G_i + P_i * C_i`
  * Ex: `C_3 = G_2 + P_1 * C_2`
  * Ex: `C_1 = G_0 + P_0 * C_0`
  * Ex: `C_2 = G_1 + P_1 * C_1`
    * But `C_1` is not yet available, so we replace it with its equation `C_2 = G_1 + P_1 * G_0 + P_0 * C_0`
    * Simplified, this means `C_2 = G_1 + P_1 G_0 + P_1 P_0 * C_0`
    * `C_3 = G_2 + G_1 P_2 + G_0 P_1 P_2 + P_0 P_1 P_3 C_0`
    * `C_4 = G3 + G2 P3 + G1 P2 P3 + G0 P1 P2 P3 + P0 P1 P2 P3 C0`
* For any `S_i = (x_i XOR y_i) XOR C_i
