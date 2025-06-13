import discord
from discord.ext import commands

intents = discord.Intents.default()
intents.message_content = True

bot = commands.Bot(command_prefix='-', intents=intents)

PROFILE_CHANNEL_ID = 1381277483554574457  # ห้องโพสต์โปรไฟล์
PRIVATE_CHANNEL_ID = 1381568733365010605  # ห้องสร้างโปรไฟล์

@bot.event
async def on_ready():
    print(f'Logged in as {bot.user}')

@bot.command()
async def pf(ctx):
    if ctx.channel.id != PRIVATE_CHANNEL_ID:
        return await ctx.send("❌ ใช้คำสั่งนี้ในห้องที่กำหนดเท่านั้นนะคะ")

    def check(m):
        return m.author == ctx.author and m.channel == ctx.channel

    await ctx.send("ขอเลขประจำตัวค่ะ")
    id_msg = await bot.wait_for('message', check=check)

    await ctx.send("ชื่อเล่น")
    name_msg = await bot.wait_for('message', check=check)

    await ctx.send("ชื่อในเกม")
    ign_msg = await bot.wait_for('message', check=check)

    await ctx.send("ชื่อเฟส")
    fb_msg = await bot.wait_for('message', check=check)

    await ctx.send("ขอโปรไฟล์เกมหน่อยค่ะ (ส่งเป็นรูปภาพ)")
    profile_pic_msg = await bot.wait_for('message', check=check)

    # ตรวจว่ามีแนบรูปไหม
    if not profile_pic_msg.attachments:
        return await ctx.send("❌ โปรดแนบรูปโปรไฟล์เกมด้วยนะคะ")

    image_url = profile_pic_msg.attachments[0].url
    user_tag = ctx.author

    # ข้อความโปรไฟล์แบบตกแต่ง
    profile_text = f"""╭ ─ ୨୧ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ╮
                     <a:whitewing1:1381270108630159460> 𝑷𝑹𝑶𝑭𝑰𝑳𝑬 <a:whitewing2:1381270024315994273> 
      เลขประจำตัว : {id_msg.content}
      ชื่อเล่น : {name_msg.content}
      ชื่อในเกม : {ign_msg.content}
      <a:1000009473:1381559143537840150>Facebook : {fb_msg.content}
      <a:emoji_102:1381560618171109386> Discord : {user_tag}
╰ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ୨୧ ─ ╯"""

    profile_channel = bot.get_channel(PROFILE_CHANNEL_ID)
    await profile_channel.send(profile_text)
    await profile_channel.send(image_url)

    await ctx.send("✅ สร้างโปรไฟล์เรียบร้อยค่ะ")

bot.run('YOUR_BOT_TOKEN')  # เปลี่ยนเป็นโทเคนของบอทเธอ
