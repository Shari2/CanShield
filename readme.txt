Hardware:
  2x NUCLEOF103RB
  2x waveshare RS485 CAN Shield
  2x wires to connect CAN H and CAN L
  2x mini USB cables to programm and communicate via debug uart


Notes:
  main_tx will wait for 8 bytes over uart2 and send it a can payload
  main_rx will listen for any can message and send playload via uart2

  can runs with 500k baud

  both applications print "wasd" over uart2 to observe resets

  binary files are generated for use with drag and drop programming

  screen /dev/ttyACM0 115200
  screen /dev/ttyACM1 115200
