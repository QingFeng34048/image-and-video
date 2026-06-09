# Robotic Arm 202606
Note: File paths for some documents need to be modified manually.
## Our Example
<div align="center">
https://github.com/qingfeng205114/image-and-video/blob/main/example.mp4
</div>

## Data Collection
`client/check_cam.py` is a script to verify whether the camera displays images normally. Test camera IDs 0, 1, 2 and 3, then record the valid ID.
`client/collect_data.py` is used for data collection. Run the script, enter a prompt (e.g., "pick up the banana") as instructed, and press Enter to start the collection process. Press S to begin recording and Y to stop recording.
`client/hdf5_to_rlds.py` converts saved HDF5 files generated after successful data collection into datasets in RLDS format. A helper script `client/run_rlds.sh` is provided for this conversion.
## Fine-Tuning
Refer to `openvla-oft/README.md` for environment setup instructions. After manually configuring all file paths, run `openvla-oft/vla-scripts/finetune.sh`. The core fine-tuning code is located at `openvla-oft/vla-scripts/finetune.py`.
## Fine-Tuning Result Validation
The Piper robotic arm acts as the client, while OpenVLA serves as the server. The two communicate over a local area network via the HTTP protocol.
Client side: Accesses the camera to capture frames, receives text prompts, packages images, prompts and robot states, and sends an HTTP POST request to the server.
Server side: Performs inference to compute robot actions and sends the action results back to the client.
In our setup, a Dell laptop running Ubuntu controls the robotic arm. Connect the two USB cables from the robotic arm and camera to the laptop.
Server code is stored in openvla-oft/server_oft.py and launched via the script `openvla-oft/run.sh`. Once started, the server stays idle and waits for data and commands sent from the client.
Next, navigate to the client folder on the client machine and execute `client/run.sh` to operate and observe the robotic arm’s movements.