import asyncio
from viam.robot.client import RobotClient
from viam.components.board import Board
from viam.components.sensor import Sensor

# Potentiometer range
POT_MAX = 1023

# Physical pin 12 supports hardware PWM
LED_PIN = "12"

# How often to check the sensor, in seconds
CHECK_INTERVAL = 0.1

async def connect():
    opts = RobotClient.Options.with_api_key(
        api_key='YOURAPIKEY',
        api_key_id='YOURAPIKEYID'
    )
    return await RobotClient.at_address('nyc-first-pi-2-main.ae8j1ofrtk.viam.cloud', opts)

async def main():
    async with await connect() as machine:
        print('Connected!')

        board = Board.from_robot(machine, "YOURBOARDNAME")
        sensor = Sensor.from_robot(machine, "mcp3008")
        led_pin = await board.gpio_pin_by_name(LED_PIN)

        print("Starting LED control loop...")

        await led_pin.set_pwm_frequency(1000)

        while True:
            readings = await sensor.get_readings()
            pot_value = readings.get("sensor")

            if pot_value is not None:
                duty_cycle = pot_value / POT_MAX
                duty_cycle = max(0.0, min(1.0, duty_cycle))

                print(f"Sensor value: {pot_value} → PWM duty cycle: {duty_cycle:.2f}")

                await led_pin.set_pwm(duty_cycle)

            await asyncio.sleep(CHECK_INTERVAL)

if __name__ == '__main__':
    asyncio.run(main())
