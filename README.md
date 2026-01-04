# RPGGGGGG - Race Selection Book (Minecraft 1.20.1)

Datapack chọn tộc bằng **sách** (click trong sách để chọn), không cần command block.

## Yêu cầu

- Minecraft: **1.20.1**
- `pack_format`: **15**

## Cài đặt

1. Copy thư mục datapack `RPGGGGGG` vào:

   `.../saves/<ten_world>/datapacks/`

2. Vào world và chạy:

   `/reload`

## Cách dùng

### Lấy sách chọn tộc

- **Cách khuyên dùng (sách dùng trigger, không bypass khóa tộc):**

  `/function rpgggggg:book/give`

- **Phát sách tự động khi vào game (mặc định 1 lần):**

  Datapack sẽ tự phát sách khi bạn vào world nếu bạn chưa chọn tộc.

### Chọn tộc

- Mở sách và bấm vào dòng **<Nhấp vào đây để chọn>**.
- Datapack sẽ dùng `/trigger chooseRace set <id>` và xử lý trong `tick`.

### Chọn tộc vĩnh viễn

- Sau khi chọn tộc, người chơi sẽ có tag `rpgg_race`.
- Khi đã có `rpgg_race`, trigger `chooseRace` **không thể đổi tộc nữa**.

> Lưu ý: Admin vẫn có thể chạy trực tiếp `/function rpgggggg:choose/<race>` để ép tộc. Đây là hành vi “admin tool”, không dành cho gameplay thường.

## Lệnh / Function

- `/function rpgggggg:book/give`
  - Phát sách chọn tộc (trigger-book).

- `/function rpgggggg:book/give_race_book_trigger`
  - Phát sách trigger-book cho `@s`.

- `/function rpgggggg:reset`
  - Reset người chơi: clear inventory + clear effect + xóa tag tộc + reset máu về 20.
  - Chỉ nên dùng cho admin/test.

## Scoreboard / Tag

### Scoreboard objectives

- `chooseRace` (trigger): nhận lựa chọn từ sách.
- `rpgg_y` (dummy): lưu Y của người chơi để xử lý logic tộc Dwarf.
- `rpgg_ender_cd` (dummy): cooldown dịch chuyển của Ender.
- Các objective khác phục vụ thống kê / nội bộ (potion used, book left...).

### Tags

- `rpgg_race`: đánh dấu đã chọn tộc.
- `rpgg_human`, `rpgg_elf`, `rpgg_dwarf`, `rpgg_orc`, `rpgg_vampire`, `rpgg_werewolf`, `rpgg_merfolk`, `rpgg_ender`, `rpgg_netherborn`.

## Cơ chế hoạt động

- `data/minecraft/tags/functions/load.json` gọi `rpgggggg:load`:
  - Tạo scoreboard cần thiết.
  - Enable trigger `chooseRace`.

- `data/minecraft/tags/functions/tick.json` gọi `rpgggggg:tick` mỗi tick:
  - Init/on-join logic.
  - Đọc lựa chọn từ `chooseRace` và gọi function chọn tộc.
  - Mỗi tick gia hạn hiệu ứng bằng `rpgggggg:main/apply`.

## Danh sách tộc (theo logic hiện tại trong datapack)

> Đây là mô tả **đúng theo code đang chạy** (không phải mô tả “ý tưởng”).

### (1) HUMAN

- Item nhận khi chọn:
  - Bread x16
  - Iron Sword
- Passive:
  - Có cơ chế thưởng EXP: mỗi lần giết mob có **5%** nhận thêm **1 exp point**.

### (2) ELF

- Máu tối đa: **18**
- Overworld:
  - Speed I (vĩnh viễn)
  - Jump Boost I (vĩnh viễn)
  - Night Vision (chỉ ban đêm)
- Nether:
  - Mất Speed/Jump
  - Weakness I
  - Không có Night Vision
- The End:
  - Có Speed/Jump
  - Không có Night Vision
- Item nhận khi chọn:
  - Bow + Arrow x32
  - Leather Armor màu xanh

### (3) DWARF

- Máu tối đa: **10**
- Hiệu ứng:
  - Haste I (vĩnh viễn)
  - Resistance I khi Y <= 39
  - Slowness I khi ở Overworld + thấy trời + đang đứng trên mặt đất
- Item:
  - Iron Pickaxe
  - Torch x32

### (4) ORC

- Hiệu ứng:
  - Strength III (gia hạn liên tục)
- Item:
  - Iron Axe
  - Cooked Beef x16

### (5) VAMPIRE

- Overworld ban ngày:
  - Weakness I
  - Không có các buff ban đêm
  - Máu tối đa 20
- Ban đêm (theo thời gian Overworld):
  - Speed III, Resistance III, Regeneration III, Jump Boost III
  - Night Vision chỉ khi ở Overworld
  - Máu tối đa 40
- Khi ở Nether / The End:
  - Có các buff như ban đêm (không phụ thuộc ngày/đêm)
  - Không có Night Vision (bị clear khi vào Nether/End)

### (6) WEREWOLF

- Ban đêm:
  - Strength III
  - Speed II
- Ban ngày:
  - Strength I

### (7) MERFOLK

- Khi ở trong nước:
  - Water Breathing, Dolphin's Grace
  - Strength II, Regeneration II, Resistance II
  - Máu tối đa 15
- Khi không ở trong nước:
  - Mất các buff trong nước
  - Weakness I
  - Máu tối đa 20
- Nether:
  - Nhận damage liên tục

### (8) ENDER

- Khi ở Overworld và ở trong nước:
  - Weakness I + nhận damage
- The End:
  - Strength III, Speed II, Regeneration I, Jump Boost II
  - Máu tối đa 20
- Passive:
  - Khi bị đánh có thể **dịch chuyển ngẫu nhiên** (spreadplayers) kèm cooldown.

### (9) NETHERBORN

- Nether:
  - Strength III, Speed II, Regeneration I, Jump Boost II
  - Fire Resistance
  - Máu tối đa 15
- Không ở Nether:
  - Mất buff
  - Máu tối đa 20
- Overworld + ở trong nước:
  - Weakness I + nhận damage

## Góp ý / Mở rộng

Nếu bạn muốn mình chỉnh cho đúng 100% theo mô tả ban đầu (ví dụ Werewolf ban ngày bị Weakness II, Orc “potion kém hiệu quả”, Ender có Night Vision vĩnh viễn...), nói mình các thay đổi bạn muốn, mình sẽ update logic tương ứng.
