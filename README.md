import flash.display.MovieClip;
import flash.events.MouseEvent;
import flash.utils.Timer;
import flash.events.TimerEvent;

class LuckyWheel extends MovieClip {
    private var wheel:MovieClip;
    private var spinning:Boolean = false;
    private var spinTimer:Timer;
    private var spinDuration:int = 3000; // 3 seconds
    private var spinAngle:int = 0;

    public function LuckyWheel() {
        wheel = new MovieClip();
        addChild(wheel);
        drawWheel();
        this.addEventListener(MouseEvent.CLICK, startSpin);
    }

    private function drawWheel():void {
        // Draw the wheel segments
        for (var i:int = 0; i < 8; i++) {
            var segment:MovieClip = new MovieClip();
            segment.graphics.beginFill(Math.random() * 0xFFFFFF);
            segment.graphics.moveTo(0, 0);
            segment.graphics.lineTo(100, 0);
            segment.graphics.lineTo(100, 100);
            segment.graphics.lineTo(0, 100);
            segment.graphics.endFill();
            segment.rotation = i * 45; // 360 / 8 segments
            wheel.addChild(segment);
        }
        wheel.x = stage.stageWidth / 2;
        wheel.y = stage.stageHeight / 2;
    }

    private function startSpin(event:MouseEvent):void {
        if (!spinning) {
            spinning = true;
            spinAngle = Math.random() * 360 + 720; // Random angle + 2 full spins
            spinTimer = new Timer(100);
            spinTimer.addEventListener(TimerEvent.TIMER, spinWheel);
            spinTimer.start();
            setTimeout(stopSpin, spinDuration);
        }
    }

    private function spinWheel(event:TimerEvent):void {
        wheel.rotation += 10; // Spin speed
    }

    private function stopSpin():void {
        spinTimer.stop();
        spinTimer.removeEventListener(TimerEvent.TIMER, spinWheel);
        spinning = false;
        wheel.rotation = wheel.rotation % 360; // Normalize rotation
        trace("Wheel stopped at: " + wheel.rotation);
    }
}
