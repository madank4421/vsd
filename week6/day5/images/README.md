% report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4
Startpoint: _31002_ (rising edge-triggered flip-flop clocked by clk)
Endpoint: _30967_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

Fanout       Cap      Slew     Delay      Time   Description
-------------------------------------------------------------------------------------
                              0.0000    0.0000   clock clk (rise edge)
                              0.0000    0.0000   clock source latency
                    0.0225    0.0100    0.0100 ^ clk (in)
     1    0.0079                                 clk (net)
                    0.0225    0.0000    0.0100 ^ clkbuf_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
                    0.0283    0.1016    0.1116 ^ clkbuf_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
     2    0.0044                                 clknet_0_clk (net)
                    0.0283    0.0000    0.1116 ^ clkbuf_1_1_0_clk/A (sky130_fd_sc_hd__clkbuf_1)
                    0.0388    0.0697    0.1814 ^ clkbuf_1_1_0_clk/X (sky130_fd_sc_hd__clkbuf_1)
     1    0.0022                                 clknet_1_1_0_clk (net)
                    0.0388    0.0000    0.1814 ^ clkbuf_1_1_1_clk/A (sky130_fd_sc_hd__clkbuf_1)
                    0.0388    0.0734    0.2547 ^ clkbuf_1_1_1_clk/X (sky130_fd_sc_hd__clkbuf_1)
     1    0.0022                                 clknet_1_1_1_clk (net)
                    0.0388    0.0000    0.2547 ^ clkbuf_1_1_2_clk/A (sky130_fd_sc_hd__clkbuf_1)
                    0.0635    0.0910    0.3458 ^ clkbuf_1_1_2_clk/X (sky130_fd_sc_hd__clkbuf_1)
     2    0.0044                                 clknet_1_1_2_clk (net)
                    0.0635    0.0000    0.3458 ^ clkbuf_2_3_0_clk/A (sky130_fd_sc_hd__clkbuf_1)
                    0.0390    0.0811    0.4268 ^ clkbuf_2_3_0_clk/X (sky130_fd_sc_hd__clkbuf_1)
     1    0.0022                                 clknet_2_3_0_clk (net)
                    0.0390    0.0000    0.4268 ^ clkbuf_2_3_1_clk/A (sky130_fd_sc_hd__clkbuf_1)
                    0.0388    0.0734    0.5003 ^ clkbuf_2_3_1_clk/X (sky130_fd_sc_hd__clkbuf_1)
     1    0.0022                                 clknet_2_3_1_clk (net)
                    0.0388    0.0000    0.5003 ^ clkbuf_2_3_2_clk/A (sky130_fd_sc_hd__clkbuf_1)
                    0.0635    0.0910    0.5913 ^ clkbuf_2_3_2_clk/X (sky130_fd_sc_hd__clkbuf_1)
     2    0.0044                                 clknet_2_3_2_clk (net)
                    0.0635    0.0000    0.5913 ^ clkbuf_3_7_0_clk/A (sky130_fd_sc_hd__clkbuf_1)
                    0.0390    0.0811    0.6724 ^ clkbuf_3_7_0_clk/X (sky130_fd_sc_hd__clkbuf_1)
     1    0.0022                                 clknet_3_7_0_clk (net)
                    0.0390    0.0000    0.6724 ^ clkbuf_3_7_1_clk/A (sky130_fd_sc_hd__clkbuf_1)
                    0.0635    0.0911    0.7635 ^ clkbuf_3_7_1_clk/X (sky130_fd_sc_hd__clkbuf_1)
     2    0.0044                                 clknet_3_7_1_clk (net)
                    0.0635    0.0000    0.7635 ^ clkbuf_4_14_0_clk/A (sky130_fd_sc_hd__clkbuf_1)
                    0.0635    0.0987    0.8622 ^ clkbuf_4_14_0_clk/X (sky130_fd_sc_hd__clkbuf_1)
     2    0.0044                                 clknet_4_14_0_clk (net)
                    0.0635    0.0000    0.8622 ^ clkbuf_5_28_0_clk/A (sky130_fd_sc_hd__clkbuf_1)
                    0.0390    0.0811    0.9433 ^ clkbuf_5_28_0_clk/X (sky130_fd_sc_hd__clkbuf_1)
     1    0.0022                                 clknet_5_28_0_clk (net)
                    0.0390    0.0000    0.9433 ^ clkbuf_5_28_1_clk/A (sky130_fd_sc_hd__clkbuf_1)
                    0.5776    0.4454    1.3886 ^ clkbuf_5_28_1_clk/X (sky130_fd_sc_hd__clkbuf_1)
     7    0.0492                                 clknet_5_28_1_clk (net)
                    0.5776    0.0000    1.3886 ^ clkbuf_leaf_104_clk/A (sky130_fd_sc_hd__clkbuf_16)
                    0.0416    0.2342    1.6229 ^ clkbuf_leaf_104_clk/X (sky130_fd_sc_hd__clkbuf_16)
     2    0.0038                                 clknet_leaf_104_clk (net)
                    0.0416    0.0000    1.6229 ^ _31002_/CLK (sky130_fd_sc_hd__dfxtp_1)
                    0.0339    0.2909    1.9138 ^ _31002_/Q (sky130_fd_sc_hd__dfxtp_1)
     1    0.0018                                 alu_add_sub[26] (net)
                    0.0339    0.0000    1.9138 ^ _29511_/A1 (sky130_fd_sc_hd__mux2_2)
                    0.0322    0.1227    2.0364 ^ _29511_/X (sky130_fd_sc_hd__mux2_2)
     1    0.0017                                 alu_out[26] (net)
                    0.0322    0.0000    2.0364 ^ _30967_/D (sky130_fd_sc_hd__dfxtp_2)
                                        2.0364   data arrival time

                              0.0000    0.0000   clock clk (rise edge)
                              0.0000    0.0000   clock source latency
                    0.0225    0.0100    0.0100 ^ clk (in)
     1    0.0079                                 clk (net)
                    0.0225    0.0000    0.0100 ^ clkbuf_0_clk/A (sky130_fd_sc_hd__clkbuf_16)
                    0.0283    0.1016    0.1116 ^ clkbuf_0_clk/X (sky130_fd_sc_hd__clkbuf_16)
     2    0.0044                                 clknet_0_clk (net)
                    0.0283    0.0000    0.1116 ^ clkbuf_1_1_0_clk/A (sky130_fd_sc_hd__clkbuf_1)
                    0.0388    0.0697    0.1814 ^ clkbuf_1_1_0_clk/X (sky130_fd_sc_hd__clkbuf_1)
     1    0.0022                                 clknet_1_1_0_clk (net)
                    0.0388    0.0000    0.1814 ^ clkbuf_1_1_1_clk/A (sky130_fd_sc_hd__clkbuf_1)
                    0.0388    0.0734    0.2547 ^ clkbuf_1_1_1_clk/X (sky130_fd_sc_hd__clkbuf_1)
     1    0.0022                                 clknet_1_1_1_clk (net)
                    0.0388    0.0000    0.2547 ^ clkbuf_1_1_2_clk/A (sky130_fd_sc_hd__clkbuf_1)
                    0.0635    0.0910    0.3458 ^ clkbuf_1_1_2_clk/X (sky130_fd_sc_hd__clkbuf_1)
     2    0.0044                                 clknet_1_1_2_clk (net)
                    0.0635    0.0000    0.3458 ^ clkbuf_2_2_0_clk/A (sky130_fd_sc_hd__clkbuf_1)
                    0.0390    0.0811    0.4268 ^ clkbuf_2_2_0_clk/X (sky130_fd_sc_hd__clkbuf_1)
     1    0.0022                                 clknet_2_2_0_clk (net)
                    0.0390    0.0000    0.4268 ^ clkbuf_2_2_1_clk/A (sky130_fd_sc_hd__clkbuf_1)
                    0.0388    0.0734    0.5003 ^ clkbuf_2_2_1_clk/X (sky130_fd_sc_hd__clkbuf_1)
     1    0.0022                                 clknet_2_2_1_clk (net)
                    0.0388    0.0000    0.5003 ^ clkbuf_2_2_2_clk/A (sky130_fd_sc_hd__clkbuf_1)
                    0.0635    0.0910    0.5913 ^ clkbuf_2_2_2_clk/X (sky130_fd_sc_hd__clkbuf_1)
     2    0.0044                                 clknet_2_2_2_clk (net)
                    0.0635    0.0000    0.5913 ^ clkbuf_3_4_0_clk/A (sky130_fd_sc_hd__clkbuf_1)
                    0.0390    0.0811    0.6724 ^ clkbuf_3_4_0_clk/X (sky130_fd_sc_hd__clkbuf_1)
     1    0.0022                                 clknet_3_4_0_clk (net)
                    0.0390    0.0000    0.6724 ^ clkbuf_3_4_1_clk/A (sky130_fd_sc_hd__clkbuf_1)
                    0.0635    0.0911    0.7635 ^ clkbuf_3_4_1_clk/X (sky130_fd_sc_hd__clkbuf_1)
     2    0.0044                                 clknet_3_4_1_clk (net)
                    0.0635    0.0000    0.7635 ^ clkbuf_4_8_0_clk/A (sky130_fd_sc_hd__clkbuf_1)
                    0.0635    0.0987    0.8622 ^ clkbuf_4_8_0_clk/X (sky130_fd_sc_hd__clkbuf_1)
     2    0.0044                                 clknet_4_8_0_clk (net)
                    0.0635    0.0000    0.8622 ^ clkbuf_5_17_0_clk/A (sky130_fd_sc_hd__clkbuf_1)
                    0.0390    0.0811    0.9433 ^ clkbuf_5_17_0_clk/X (sky130_fd_sc_hd__clkbuf_1)
     1    0.0022                                 clknet_5_17_0_clk (net)
                    0.0390    0.0000    0.9433 ^ clkbuf_5_17_1_clk/A (sky130_fd_sc_hd__clkbuf_1)
                    1.0082    0.7429    1.6862 ^ clkbuf_5_17_1_clk/X (sky130_fd_sc_hd__clkbuf_1)
    11    0.0868                                 clknet_5_17_1_clk (net)
                    1.0082    0.0000    1.6862 ^ clkbuf_leaf_170_clk/A (sky130_fd_sc_hd__clkbuf_16)
                    0.0688    0.2991    1.9853 ^ clkbuf_leaf_170_clk/X (sky130_fd_sc_hd__clkbuf_16)
    12    0.0225                                 clknet_leaf_170_clk (net)
                    0.0688    0.0000    1.9853 ^ _30967_/CLK (sky130_fd_sc_hd__dfxtp_2)
                              0.0000    1.9853   clock reconvergence pessimism
                             -0.0254    1.9598   library hold time
                                        1.9598   data required time
-------------------------------------------------------------------------------------
                                        1.9598   data required time
                                       -2.0364   data arrival time
-------------------------------------------------------------------------------------
                                        0.0766   slack (MET)


Startpoint: resetn (input port clocked by clk)
Endpoint: mem_la_read (output port clocked by clk)
Path Group: clk
Path Type: max

Fanout       Cap      Slew     Delay      Time   Description
-------------------------------------------------------------------------------------
                              0.0000    0.0000   clock clk (rise edge)
                              0.0000    0.0000   clock network delay (propagated)
                              4.9460    4.9460 ^ input external delay
                    0.0182    0.0064    4.9524 ^ resetn (in)
     1    0.0049                                 resetn (net)
                    0.0182    0.0000    4.9524 ^ input101/A (sky130_fd_sc_hd__buf_6)
                    0.0587    0.1008    5.0532 ^ input101/X (sky130_fd_sc_hd__buf_6)
     7    0.0234                                 net101 (net)
                    0.0587    0.0000    5.0532 ^ _15304_/A (sky130_fd_sc_hd__clkbuf_4)
                    0.0934    0.1738    5.2269 ^ _15304_/X (sky130_fd_sc_hd__clkbuf_4)
     6    0.0265                                 _12638_ (net)
                    0.0934    0.0000    5.2269 ^ _17093_/C (sky130_fd_sc_hd__nand3_4)
                    0.1284    0.1274    5.3543 v _17093_/Y (sky130_fd_sc_hd__nand3_4)
     4    0.0282                                 _13857_ (net)
                    0.1284    0.0000    5.3543 v _18867_/B1 (sky130_fd_sc_hd__a21oi_4)
                    0.0799    0.1239    5.4782 ^ _18867_/Y (sky130_fd_sc_hd__a21oi_4)
     1    0.0023                                 net199 (net)
                    0.0799    0.0000    5.4782 ^ output199/A (sky130_fd_sc_hd__clkbuf_2)
                    0.1052    0.1596    5.6378 ^ output199/X (sky130_fd_sc_hd__clkbuf_2)
     1    0.0177                                 mem_la_read (net)
                    0.1052    0.0000    5.6378 ^ mem_la_read (out)
                                        5.6378   data arrival time

                             24.7300   24.7300   clock clk (rise edge)
                              0.0000   24.7300   clock network delay (propagated)
                              0.0000   24.7300   clock reconvergence pessimism
                             -4.9460   19.7840   output external delay
                                       19.7840   data required time
-------------------------------------------------------------------------------------
                                       19.7840   data required time
                                       -5.6378   data arrival time
-------------------------------------------------------------------------------------
                                       14.1462   slack (MET)



