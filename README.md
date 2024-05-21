# Goal of this project
Count the number of unique people in a video.

# Limitations
1. Won't work from RTSP streams, only from a video file.
2. Wasted computation, as it has 2 separate models for tracking and ReID. The model that generates the appearance feature embeddings for the internal association step of the tracker could be the same as the ReID model, but in this implementation they're not, for code clarity.
3. Performance (FPS) is not optimized.
4. The video must end to finish the count, so it's not a real time system.

# Approach
1. Detect all people in every frame with a YOLOv8 object detector. Using size X for best performance.
2. Track people across frames using ByteTrack. This generates at least one tracklet per person, but if a person leaves the scene for a long time and comes back, multiple track IDs for the same person will be generated.
3. For every tracklet, generate an appearance feature embedding using a ReID network (OSNet). The mean embedding across all the bounding boxes (across multiple frames) is chosen as the representation of the appearance of a person within that tracklet.
4. Cluster all the appearance feature embeddings. The number of clusters should indicate the number of unique people.

# Code
The notebook was tested in a Google Colab with a T4 instance.
