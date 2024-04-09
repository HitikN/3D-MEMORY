# 3D-MEMORY
Project Title: 3D Content-Addressable Memory (CAM) Design
------------------------
CODE ===>

module cam_3d (
    input wire [DATA_WIDTH-1:0] search_data,
    input wire [ADDRESS_DEPTH-1:0] search_address_x,
    input wire [ADDRESS_DEPTH-1:0] search_address_y,
    input wire [ADDRESS_DEPTH-1:0] search_address_z,
    input wire write_enable,
    input wire [DATA_WIDTH-1:0] write_data,
    output reg [ADDRESS_DEPTH-1:0] match_x,
    output reg [ADDRESS_DEPTH-1:0] match_y,
    output reg [ADDRESS_DEPTH-1:0] match_z,
    output reg match_found
);

parameter DATA_WIDTH = 8; // Width of each memory word
parameter ADDRESS_DEPTH = 4; // Depth of 3D memory array

reg [DATA_WIDTH-1:0] memory [0:2**ADDRESS_DEPTH-1][0:2**ADDRESS_DEPTH-1][0:2**ADDRESS_DEPTH-1];

always @ (posedge clk) begin
    // Write operation
    if (write_enable) begin
        memory[search_address_x][search_address_y][search_address_z] <= write_data;
    end

    // Search operation
    match_found = 0;
    for (int i = 0; i < 2**ADDRESS_DEPTH; i = i + 1) begin
        for (int j = 0; j < 2**ADDRESS_DEPTH; j = j + 1) begin
            for (int k = 0; k < 2**ADDRESS_DEPTH; k = k + 1) begin
                if (memory[i][j][k] == search_data) begin
                    match_x = i;
                    match_y = j;
                    match_z = k;
                    match_found = 1;
                end
            end
        end
    end
end

endmodule
