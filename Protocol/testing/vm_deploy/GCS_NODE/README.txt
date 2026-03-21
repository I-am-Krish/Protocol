UAVLink --- GCS NODE Bundle (Fedora Linux)
=========================================

WHAT THIS IS:
  This machine acts as the Ground Control Station.
  It receives live telemetry from the UAV and sends
  encrypted, authenticated commands back to it.

QUICK START (3 commands):
  1. sudo dnf install gcc -y          # install compiler (once)
  2. chmod +x build.sh run_gcs.sh && ./build.sh
  3. ./run_gcs.sh <UAV_VM_IP>

FIREWALL (run once as root):
  sudo firewall-cmd --add-port=14552/udp --permanent
  sudo firewall-cmd --reload

FILES IN THIS BUNDLE:
  gcs_receiver.c       Main GCS source
  *.c / *.h            Protocol library source
  gcs_id_seed.bin      GCS private identity (keep secret)
  uav_pub.bin          UAV public key (authenticates UAV = no MITM)
  build.sh             Compiles the GCS binary
  run_gcs.sh           Launches the GCS process

GCS COMMANDS (press key in terminal after ECDH established):
  1=ARM  2=DISARM  3=TAKEOFF  4=LAND  5=RTL  6=EMERGENCY
  7=Set Flight Mode  8=Send Waypoint  9=Upload Mission Plan
  0=Show Menu

WHAT HAPPENS AT LINK-UP:
  - GCS sends its ECDH public key (ephemeral, fresh each run)
  - UAV verifies GCS identity via EdDSA (gcs_id_seed.bin)
  - Session key auto-derived (ChaCha20-Poly1305 AEAD)
  - All commands are encrypted + MAC-authenticated
