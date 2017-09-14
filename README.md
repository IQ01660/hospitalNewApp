# hospitalNewAppNotFinished

``` C#
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using System.Data.SqlClient;

namespace HospitalDbApp
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void Form1_Load(object sender, EventArgs e)
        {
            
        }
        // section part
        private void button1_Click(object sender, EventArgs e)
        {
            Controls.Clear();
            Button backBtn = new Button();
            Button enterBtn = new Button();
            Controls.Add(backBtn);
            Controls.Add(enterBtn);
            textBox1.Left = 250;
            textBox1.Top = 100;
            textBox1.Size = new Size(140, 50);
            Controls.Add(textBox1);
            backBtn.Top = 300;
            backBtn.Left = 250;
            enterBtn.Top = 300;
            enterBtn.Left = 400;
            enterBtn.Text = "Enter";
            backBtn.Text = "Back";
            backBtn.Click += new System.EventHandler(this.backBtn_Click);
            enterBtn.Click += new System.EventHandler(this.enterBtn_Click);
            MessageBox.Show("Enter the section name");
        }
        public void enterBtn_Click(object sender,EventArgs e)
        {
            var newSectionName = textBox1.Text;
            var connectionString = @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=HospitalApp;Integrated Security=True;Connect Timeout=15;Encrypt=False;TrustServerCertificate=True;ApplicationIntent=ReadWrite;MultiSubnetFailover=False";
            var conn = new SqlConnection(connectionString);
            conn.Open();
            var queryString = "Insert into [dbo].[Section] (Name) values('" + newSectionName + "')";
            SqlCommand insertSection = new SqlCommand(queryString, conn);
            insertSection.ExecuteNonQuery();
            conn.Close();
            MessageBox.Show("OK");
        }
        public void backBtn_Click(object sender, EventArgs e) // click methods should be added
        {
            Button button1 = new Button();
            Button button2 = new Button();
            Button button3 = new Button();
            Button button4 = new Button();
            Controls.Clear();
            Controls.Add(button1);
            Controls.Add(button2);
            Controls.Add(button3);
            Controls.Add(button4);
            // 
            // button1
            // 
            button1.Location = new System.Drawing.Point(336, 97);
            button1.Name = "button1";
            button1.Size = new System.Drawing.Size(118, 41);
            button1.TabIndex = 0;
            button1.Text = "Section";
            button1.UseVisualStyleBackColor = true;
            button1.Click += new System.EventHandler(this.button1_Click);
            // 
            // button2
            // 
            button2.Location = new System.Drawing.Point(460, 97);
            button2.Name = "button2";
            button2.Size = new System.Drawing.Size(118, 41);
            button2.TabIndex = 1;
            button2.Text = "Doctor";
            button2.UseVisualStyleBackColor = true;
            button2.Click += new System.EventHandler(this.button2_Click);
            // 
            // button3
            // 
            button3.Location = new System.Drawing.Point(584, 97);
            button3.Name = "button3";
            button3.Size = new System.Drawing.Size(118, 41);
            button3.TabIndex = 2;
            button3.Text = "Days";
            button3.UseVisualStyleBackColor = true;
            // 
            // button4
            // 
            button4.Location = new System.Drawing.Point(708, 97);
            button4.Name = "button4";
            button4.Size = new System.Drawing.Size(118, 41);
            button4.TabIndex = 3;
            button4.Text = "Queue";
            button4.UseVisualStyleBackColor = true;
        }
        // doctor part
        private void button2_Click(object sender, EventArgs e)
        {
            Controls.Clear();
            Button backBtn2 = new Button();
            Button enterBtn2 = new Button();
            Controls.Add(backBtn2);
            Controls.Add(enterBtn2);
            textBox2.Size = new Size(140,20);
            textBox3.Size = new Size(140,20);
            comboBox1.Size = new Size(140, 21);
            Controls.Add(textBox2);
            Controls.Add(textBox3);
            Controls.Add(comboBox1);
            backBtn2.Top = 300;
            backBtn2.Left = 250;
            enterBtn2.Top = 300;
            enterBtn2.Left = 400;
            enterBtn2.Text = "Enter";
            backBtn2.Text = "Back";
            backBtn2.Click += new System.EventHandler(this.backBtn_Click);
            enterBtn2.Click += new System.EventHandler(this.enterBtn2_Click);

            var connectionString2 = @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=HospitalApp;Integrated Security=True;Connect Timeout=15;Encrypt=False;TrustServerCertificate=True;ApplicationIntent=ReadWrite;MultiSubnetFailover=False";
            var conn2 = new SqlConnection(connectionString2);
            conn2.Open();
            var queryString2 = "Select * from [dbo].[Section];";
            SqlCommand insertSectionsInCombo = new SqlCommand(queryString2, conn2);
            var adapter = new SqlDataAdapter(insertSectionsInCombo);
            var dataset = new DataSet();
            adapter.Fill(dataset);
            conn2.Close();
            
            for (int i = 0; i < dataset.Tables[0].Rows.Count; i++)
            {
                comboBox1.Items.Add(dataset.Tables[0].Rows[i]["Name"].ToString());
            }
        }
        public void enterBtn2_Click(object sender, EventArgs e)
        {
            var sectionNameOfDoctor = comboBox1.SelectedItem.ToString();
           // var findSectionIdOfDoctor = "Select "
            //var newDoctorName = textBox2.Text;
            //var newDoctorSurname = textBox3.Text;
            //var connectionString = @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=HospitalApp;Integrated Security=True;Connect Timeout=15;Encrypt=False;TrustServerCertificate=True;ApplicationIntent=ReadWrite;MultiSubnetFailover=False";
            //var conn = new SqlConnection(connectionString);
            //conn.Open();
           // var queryString = "Insert into [dbo].[Doctor] (Name,Surname,Section_Id) values('" + newDoctorName + "','"+newDoctorSurname+"','"+"')";
           // SqlCommand insertSection = new SqlCommand(queryString, conn);
         //   insertSection.ExecuteNonQuery();
          //  conn.Close();
            MessageBox.Show("OK");
        }
    }
}

```
