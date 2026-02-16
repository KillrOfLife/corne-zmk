Good question — short answer: keep only the node that matches the actual wiring (disable the unused one).

Why: both the I2C and SPI ssd1306 nodes use the same binding/compatible so the driver will instantiate for every matching node. You can’t just give the SPI instance a different compatible (that would break the driver). If both nodes are present the build will try to compile driver support for both bus types and you get the DT/C compile errors.

What I did is the correct approach for an SPI-wired display on a board that also provides an I2C OLED: disable the I2C node and provide a single SPI node (or vice‑versa if your hardware is I2C).

Alternatives:
- If your hardware is actually I2C: remove the SPI node and leave I2C enabled (and revert the `CONFIG_SPI` change).
- If your hardware is SPI: keep the SPI node and keep the I2C node disabled (what I applied).
- You can also disable the board/overlay I2C node upstream (in the shield/board overlay) instead of in your `corne.keymap` overlay — either works.

Next step question for you:
- Revert to I2C (remove SPI changes), or
- Keep SPI and I2C disabled as currently configured?
