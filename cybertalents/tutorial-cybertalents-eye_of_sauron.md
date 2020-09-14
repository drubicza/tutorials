## [Writeup] Cyber Talents: Eye Of Sauron


[Soal ini](https://s3-eu-west-1.amazonaws.com/talentchallenges/Reverse/Inkie.zip) berada pada kategori _easy_ dan petunjuknya adalah sebagai berikut:
```
Can you find the key to pass?
```

Adapun soalnya adalah sebuah aplikasi dotnet dengan nama `Inkie.exe`. Gunakan ILSpy untuk melakukan dekompilasi terhadap aplikasi tersebut, maka hasilnya seperti ini:
```cs
using System;
using System.ComponentModel;
using System.Drawing;
using System.Windows.Forms;
namespace Inkie
{
    public class Form1 : Form
    {
        private IContainer components;
        private Label label1;
        private TextBox txtPass;
        private Button btnCheck;
        private Label label2;
        private Label label3;
        private Label label4;
        private Label label5;
        public Form1()
        {
            this.InitializeComponent();
        }
        private string reverse(string original)
        {
            char[] array = original.ToCharArray();
            Array.Reverse(array);
            return new string(array);
        }
        private bool ShallHePass()
        {
            return this.txtPass.Text == this.reverse(this.label2.Text + this.label3.Text + this.label4.Text + this.label5.Text);
        }
        private void btnCheck_Click(object sender, EventArgs e)
        {
            this.label3.Text = "47996655";
            MessageBox.Show(this.ShallHePass() ? "you shall pass" : "you shall not pass");
        }
        private void Form1_Load(object sender, EventArgs e)
        {
            this.label4.Text = "83f05689";
        }
        protected override void Dispose(bool disposing)
        {
            if (disposing && this.components != null)
            {
                this.components.Dispose();
            }
            base.Dispose(disposing);
        }
        private void InitializeComponent()
        {
            this.label1 = new Label();
            this.txtPass = new TextBox();
            this.btnCheck = new Button();
            this.label2 = new Label();
            this.label3 = new Label();
            this.label4 = new Label();
            this.label5 = new Label();
            base.SuspendLayout();
            this.label1.AutoSize = true;
            this.label1.Location = new Point(12, 80);
            this.label1.Name = "label1";
            this.label1.Size = new Size(131, 13);
            this.label1.TabIndex = 0;
            this.label1.Text = "Enter the key on the ring:";
            this.txtPass.Location = new Point(143, 77);
            this.txtPass.Name = "txtPass";
            this.txtPass.Size = new Size(129, 20);
            this.txtPass.TabIndex = 1;
            this.btnCheck.Location = new Point(105, 125);
            this.btnCheck.Name = "btnCheck";
            this.btnCheck.Size = new Size(75, 23);
            this.btnCheck.TabIndex = 2;
            this.btnCheck.Text = "I shall Pass!";
            this.btnCheck.UseVisualStyleBackColor = true;
            this.btnCheck.Click += new EventHandler(this.btnCheck_Click);
            this.label2.AutoSize = true;
            this.label2.ForeColor = SystemColors.Control;
            this.label2.Location = new Point(12, 165);
            this.label2.Name = "label2";
            this.label2.Size = new Size(55, 13);
            this.label2.TabIndex = 3;
            this.label2.Text = "d0248b4e";
            this.label3.AutoSize = true;
            this.label3.ForeColor = SystemColors.Control;
            this.label3.Location = new Point(12, 178);
            this.label3.Name = "label3";
            this.label3.Size = new Size(55, 13);
            this.label3.TabIndex = 4;
            this.label3.Text = "47886655";
            this.label4.AutoSize = true;
            this.label4.ForeColor = SystemColors.Control;
            this.label4.Location = new Point(12, 191);
            this.label4.Name = "label4";
            this.label4.Size = new Size(53, 13);
            this.label4.TabIndex = 5;
            this.label4.Text = "83f05688";
            this.label5.AutoSize = true;
            this.label5.ForeColor = SystemColors.Control;
            this.label5.Location = new Point(12, 204);
            this.label5.Name = "label5";
            this.label5.Size = new Size(54, 13);
            this.label5.TabIndex = 6;
            this.label5.Text = "c154b6ea";
            base.AutoScaleDimensions = new SizeF(6f, 13f);
            base.AutoScaleMode = AutoScaleMode.Font;
            base.ClientSize = new Size(284, 262);
            base.Controls.Add(this.label5);
            base.Controls.Add(this.label4);
            base.Controls.Add(this.label3);
            base.Controls.Add(this.label2);
            base.Controls.Add(this.btnCheck);
            base.Controls.Add(this.txtPass);
            base.Controls.Add(this.label1);
            base.FormBorderStyle = FormBorderStyle.FixedSingle;
            base.MaximizeBox = false;
            base.MinimizeBox = false;
            base.Name = "Form1";
            this.Text = "Eyes of Sauron";
            base.Load += new EventHandler(this.Form1_Load);
            base.ResumeLayout(false);
            base.PerformLayout();
        }
    }
}
```

Perhatikan bagian ini:
```cs
private bool ShallHePass()
{
    return this.txtPass.Text == this.reverse(this.label2.Text + this.label3.Text + this.label4.Text + this.label5.Text);
}
```

Pada bagian tersebut, ada beberapa teks yang digabungkan lalu dibalik. Berikut ini adalah teks yang dimaksud:
```cs
this.label2.Text = "d0248b4e";
this.label3.Text = "47886655";
this.label4.Text = "83f05688";
this.label5.Text = "c154b6ea";
```

Namun tidak semudah itu, karena beberapa diantara teks tersebut akan berubah. Ketika form aplikasi di-load, maka teks pada `label4` akan berubah:
```cs
private void Form1_Load(object sender, EventArgs e)
{
    this.label4.Text = "83f05689";
}
```

Demikian pula ketika `BtnCheck` diklik, maka teks pada `label3` akan berubah:
```cs
private void btnCheck_Click(object sender, EventArgs e)
{
    this.label3.Text = "47996655";
    MessageBox.Show(this.ShallHePass() ? "you shall pass" : "you shall not pass");
}
```

Dari informasi di atas, maka kita dapat menyimpulkan bahwa nilai akhir untuk setiap teks adalah sebagai berikut:
```cs
this.label2.Text = "d0248b4e";
this.label3.Text = "47996655";
this.label4.Text = "83f05689";
this.label5.Text = "c154b6ea";
```

Sehingga jika digabungkan dan dibalik maka hasilnya adalah:
```bash
echo "d0248b4e4799665583f05689c154b6ea" | rev
ae6b451c98650f3855669974e4b8420d
```

Flagnya adalah: `ae6b451c98650f3855669974e4b8420d`. Sekian tutorial kali ini, semoga bermanfaat. Terima kasih kepada Tuhan Yang Maha Esa, dan Anda yang telah membaca tutorial ini.
