UAVLink --- UAV NODE Bundle (Fedora Linux)
=========================================

WHAT THIS IS:
  This machine acts as the UAV (drone).
  It sends real telemetry (GPS, attitude, heartbeat) via UDP
  and receives encrypted, authenticated commands from the GCS.

QUICK START (3 commands):
  1. sudo dnf install gcc -y          # install compiler (once)
  2. chmod +x build.sh run_uav.sh && ./build.sh
  3. ./run_uav.sh <GCS_VM_IP>

FIREWALL (run once as root):
  sudo firewall-cmd --add-port=14553/udp --permanent
  sudo firewall-cmd --reload

FILES IN THIS BUNDLE:
  uav_simulator.c      Main UAV source
  *.c / *.h            Protocol library source
  uav_id_seed.bin      UAV private identity (keep secret)
  gcs_pub.bin          GCS public key (authenticates GCS = no MITM)
  build.sh             Compiles the UAV binary
  run_uav.sh           Launches the UAV process

WHAT HAPPENS AT LINK-UP:
  - UAV sends its ECDH public key (ephemeral, fresh each run)
  - GCS verifies UAV identity via EdDSA (uav_id_seed.bin)
  - Session key auto-derived (ChaCha20-Poly1305 AEAD)
  - All telemetry and commands are encrypted from this point
