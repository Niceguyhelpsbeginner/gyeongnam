import cv2
from djitellopy import Tello

# 드론 객체 생성
def delivery():
    tello = Tello()
    tello.connect()
    tello.streamon()
    qr_detector = cv2.QRCodeDetector()
    tello.takeoff()
    try:
        while True:
            frame = tello.get_frame_read().frame
            data, bbox, _ = qr_detector.detectAndDecode(frame)
            
            if bbox is not None:
                # bbox는 4개의 꼭짓점이 있는 배열 형태 (N x 1 x 2)
                pts = bbox[0].astype(int)  # (4, 2)
                for i in range(len(pts)):
                    pt1 = tuple(pts[i])
                    pt2 = tuple(pts[(i + 1) % len(pts)])
                    cv2.line(frame, pt1, pt2, (0, 255, 0), 2)

                # QR 내용도 표시
                if data:
                    cv2.putText(frame, data, tuple(pts[0]), cv2.FONT_HERSHEY_SIMPLEX, 0.6, (255, 0, 0), 2)
                    if "1GemR" in data:
                        tello.land() 
                else:
                    tello.move_forward()
            cv2.imshow("Tello Camera", frame)

            if cv2.waitKey(1) & 0xFF == ord('q'):  # 'q' 누르면 종료
                break

    finally:
        tello.streamoff()
        cv2.destroyAllWindows()
