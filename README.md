# Object Detection Service

Welcome to the Object Detection Service repository! This service lets you detect objects in images using YOLOv5, all through a Telegram bot interface.

## How Does it Work

1. **Send Image to Telegram Bot**: Start by sending an image to the Telegram bot.
2. **Image Processing**: The Telegram bot forwards the image to YOLOv5 for object detection.
3. **Object Detection**: YOLOv5 processes the image and identifies objects present in it.
4. **Return Results**: The detected objects are sent back to the Telegram bot.
5. **Receive Results**: The Telegram bot then returns the identified objects to the user.

## Dependencies

- [Python 3](https://www.python.org/)
- [YOLOv5](https://github.com/ultralytics/yolov5)
- [Telegram API](https://core.telegram.org/bots/api)


