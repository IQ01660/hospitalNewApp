# hospitalNewApp

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

namespace HospitalAppwithDB
{
    public partial class Form1 : Form
    {
        public string btnStatus = "None";
        public Form1()
        {
            InitializeComponent();
        }

        private void Form1_Load(object sender, EventArgs e)
        {

        }
        // Section Part
        private void sectionBtn_Click(object sender, EventArgs e)
        {
            btnStatus = "Section";
            sectionBtn.Size = new Size(0,51);
            doctorBtn.Size = new Size(0, 51);
            daysBtn.Size = new Size(0, 51);
            sectionNameInput.Size = new Size(170,20);
            sectionNameInput.Text = "Section Name";
            backBtn.Size = new Size(70,23);
            enterBtn.Size = new Size(70,23);
        }
        // Doctor Part
        private void doctorBtn_Click(object sender, EventArgs e)
        {
            btnStatus = "Doctor";
            sectionBtn.Size = new Size(0, 51);
            doctorBtn.Size = new Size(0, 51);
            daysBtn.Size = new Size(0, 51);
            doctorNameInput.Size = new Size(134,20);
            doctorSurnameInput.Size = new Size(134, 20);
            availableSections.Size = new Size(134, 20);
            availableDays.Size = new Size(134, 20);
            availableQueue.Size = new Size(134, 20);
            backBtn.Size = new Size(70, 23);
            enterBtn.Size = new Size(70, 23);
            // adding elements to the comboboxes
            string connectionString = @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=HospitalApp;Integrated Security=True;Connect Timeout=30;Encrypt=False;TrustServerCertificate=True;ApplicationIntent=ReadWrite;MultiSubnetFailover=False";
            SqlConnection connection = new SqlConnection(connectionString);
            connection.Open();
            // filling sections
            string queryStringSection = "select * from [dbo].[Section]";
            SqlCommand commandSection = new SqlCommand(queryStringSection,connection);
            var adapterSection = new SqlDataAdapter(commandSection);
            var datasetSection = new DataSet();
            adapterSection.Fill(datasetSection);
            for (int i = 0; i < datasetSection.Tables[0].Rows.Count; i++)
            {
                availableSections.Items.Add(datasetSection.Tables[0].Rows[i]["Name"].ToString());
            }
            // filling days
            string queryStringDays = "select * from [dbo].[Days]";
            SqlCommand commandDays = new SqlCommand(queryStringDays,connection);
            var adapterDays = new SqlDataAdapter(commandDays);
            var datasetDays = new DataSet();
            adapterDays.Fill(datasetDays);
            for (int i = 0; i < datasetDays.Tables[0].Rows.Count; i++)
            {
                availableDays.Items.Add(datasetDays.Tables[0].Rows[i]["Date"].ToString());
            }
            //filling queue
            string queryStringQueue = "select * from [dbo].[Queue]";
            SqlCommand commandQueue = new SqlCommand(queryStringQueue, connection);
            var adapterQueue = new SqlDataAdapter(commandQueue);
            var datasetQueue = new DataSet();
            adapterQueue.Fill(datasetQueue);
            for (int i = 0; i < datasetQueue.Tables[0].Rows.Count; i++)
            {
                availableQueue.Items.Add(datasetQueue.Tables[0].Rows[i]["Name"].ToString());
            }
        }
        // DAYS BUTTON PART
        private void daysBtn_Click(object sender, EventArgs e)
        {
            btnStatus = "Days";
            sectionBtn.Size = new Size(0, 51);
            doctorBtn.Size = new Size(0, 51);
            daysBtn.Size = new Size(0, 51);
            daysInput.Size = new Size(134,20);
            backBtn.Size = new Size(70, 23);
            enterBtn.Size = new Size(70, 23);
        }
        // enter button
        private void enterBtn_Click(object sender, EventArgs e)
        {
            if (btnStatus == "Section")
            {
                string newSectionName = sectionNameInput.Text;
                string connectionString = @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=HospitalApp;Integrated Security=True;Connect Timeout=30;Encrypt=False;TrustServerCertificate=True;ApplicationIntent=ReadWrite;MultiSubnetFailover=False";
                SqlConnection connection = new SqlConnection(connectionString);
                connection.Open();
                string queryString = "insert into [dbo].[Section] (Name) values('"+newSectionName+"');";
                SqlCommand command = new SqlCommand(queryString,connection);
                command.ExecuteNonQuery();
                connection.Close();
                MessageBox.Show("The New Section: "+newSectionName+" is added to the database");
            }
            if (btnStatus == "Doctor")
            {
                string newDoctorName = doctorNameInput.Text;
                string newDoctorSurname = doctorSurnameInput.Text;
                string newDoctorSection = availableSections.SelectedItem.ToString();
                string newDoctorDays = availableDays.SelectedItem.ToString();
                string newDoctorQueue = availableQueue.SelectedItem.ToString();
                string connectionString = @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=HospitalApp;Integrated Security=True;Connect Timeout=30;Encrypt=False;TrustServerCertificate=True;ApplicationIntent=ReadWrite;MultiSubnetFailover=False";
                SqlConnection connection = new SqlConnection(connectionString);
                connection.Open();
                string queryStringGetSectionId = "select Id from [dbo].[Section] where Name = '"+newDoctorSection+"'";
                SqlCommand GetSectionId = new SqlCommand(queryStringGetSectionId,connection);
                SqlDataReader readerSection = GetSectionId.ExecuteReader();
                
                while (readerSection.Read())
                {
                   
                    int selectedSectionIdString = readerSection.GetInt32(0);
                   // MessageBox.Show("OK");
                    SectionIdHolder.sectionIdList.Add(selectedSectionIdString);
                    
                }
                //string selectedSectionIdString = getSectionIdDataset.Tables[0].Rows[0]["Id"].ToString();
                int selectedSectionId = Convert.ToInt32(SectionIdHolder.sectionIdList[0]);
                //MessageBox.Show(selectedSectionId.ToString
                connection.Close();
                string connectionString0 = @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=HospitalApp;Integrated Security=True;Connect Timeout=30;Encrypt=False;TrustServerCertificate=True;ApplicationIntent=ReadWrite;MultiSubnetFailover=False";
                SqlConnection connection0 = new SqlConnection(connectionString0);
                connection0.Open();
                string queryStringDoctorInsert = "insert into [dbo].[Doctor] (Name,Surname,Section_Id) values('"+newDoctorName+"','"+newDoctorSurname+"',"+ selectedSectionId +")";
                //adding the doctor to pivotal table
                SqlCommand DoctorInsert = new SqlCommand(queryStringDoctorInsert,connection0);
                DoctorInsert.ExecuteNonQuery();
                // MessageBox.Show("INSERED");
                connection0.Close();
                //  finding the id of the doctor
                string connectionString2 = @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=HospitalApp;Integrated Security=True;Connect Timeout=30;Encrypt=False;TrustServerCertificate=True;ApplicationIntent=ReadWrite;MultiSubnetFailover=False";
                SqlConnection connection2 = new SqlConnection(connectionString2);
                connection2.Open();
                string queryStringGetDoctorId = "select Id from [dbo].[Doctor] where Name = '"+newDoctorName+"' and Surname = '"+newDoctorSurname+"'";
                SqlCommand GetDoctorId = new SqlCommand(queryStringGetDoctorId, connection2);
                //MessageBox.Show("OK");
                SqlDataReader readerDoctor = GetDoctorId.ExecuteReader();

                while (readerDoctor.Read())
                {
                   // MessageBox.Show("OK");
                    int selectedDoctorIdString = readerDoctor.GetInt32(0);
                     //MessageBox.Show("OK");
                    DoctorIdHolder.doctorIdList.Add(selectedDoctorIdString);
                    //MessageBox.Show(DoctorIdHolder.doctorIdList[0].ToString());

                }
                // string selectedDoctorIdString = getDoctorIdDataset.Tables[0].Rows[0]["Id"].ToString();
               // MessageBox.Show("OK");
                int selectedDoctorId = Convert.ToInt32(DoctorIdHolder.doctorIdList[0]); // the doctor id
                //MessageBox.Show("OK");
                connection2.Close();
                //  finding the id of the day
                string connectionString3 = @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=HospitalApp;Integrated Security=True;Connect Timeout=30;Encrypt=False;TrustServerCertificate=True;ApplicationIntent=ReadWrite;MultiSubnetFailover=False";
                SqlConnection connection3 = new SqlConnection(connectionString3);
                connection3.Open();
                string queryStringGetDaysId = "select Id from [dbo].[Days] where Date = '" + newDoctorDays + "'";
                SqlCommand GetDaysId = new SqlCommand(queryStringGetDaysId, connection3);
                SqlDataReader readerDays = GetDaysId.ExecuteReader();
                while (readerDays.Read())
                {
                    // MessageBox.Show("OK");
                    int selectedDaysIdString = readerDays.GetInt32(0);
                    //MessageBox.Show("OK");
                    DaysIdHolder.daysIdList.Add(selectedDaysIdString);
                    //MessageBox.Show(DoctorIdHolder.doctorIdList[0].ToString());

                }
                int selectedDaysId = Convert.ToInt32(DaysIdHolder.daysIdList[0]); // the days id
                connection3.Close();
                // MessageBox.Show("OKKK");
                //  finding the id of the queue
                MessageBox.Show("2");
                string connectionString4 = @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=HospitalApp;Integrated Security=True;Connect Timeout=30;Encrypt=False;TrustServerCertificate=True;ApplicationIntent=ReadWrite;MultiSubnetFailover=False";
                SqlConnection connection4 = new SqlConnection(connectionString4);
                connection4.Open();
                string queryStringGetQueueId = "select Id from [dbo].[Queue] where Name = '" + newDoctorQueue + "'";
                SqlCommand GetQueueId = new SqlCommand(queryStringGetQueueId, connection4);
                SqlDataReader readerQueue = GetQueueId.ExecuteReader();
                while (readerQueue.Read())
                {
                    // MessageBox.Show("OK");
                    int selectedQueueIdString = readerQueue.GetInt32(0);
                    //MessageBox.Show("OK");
                    QueueIdHolder.queueIdList.Add(selectedQueueIdString);
                    //MessageBox.Show(DoctorIdHolder.doctorIdList[0].ToString());

                }
                int selectedQueueId = Convert.ToInt32(QueueIdHolder.queueIdList[0]); // the days id
                connection4.Close();
                //eventually inserting the data into the pivotal table
                MessageBox.Show("2");
                string connectionString5 = @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=HospitalApp;Integrated Security=True;Connect Timeout=30;Encrypt=False;TrustServerCertificate=True;ApplicationIntent=ReadWrite;MultiSubnetFailover=False";
                SqlConnection connection5 = new SqlConnection(connectionString5);
                connection5.Open();
                string queryInsertIntoPivotal = "insert into [dbo].[Pivotal] (Doctor_Id,Days_Id,Queue_Id) values("+selectedDoctorId+","+selectedDaysId+","+selectedQueueId+")";
                SqlCommand InsertFinalIntoPivotal = new SqlCommand(queryInsertIntoPivotal,connection5);
                InsertFinalIntoPivotal.ExecuteNonQuery();
                connection5.Close();
                MessageBox.Show("Doctor " +newDoctorSurname+ " MD. is now working in the clinic" );
            }
            // Days part
            if (btnStatus == "Days")
            {
                string newDaysString = daysInput.Text;
                string connectionString6 = @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=HospitalApp;Integrated Security=True;Connect Timeout=30;Encrypt=False;TrustServerCertificate=True;ApplicationIntent=ReadWrite;MultiSubnetFailover=False";
                SqlConnection connection6 = new SqlConnection(connectionString6);
                connection6.Open();
                string queryInsertDay = "insert into [dbo].[Days] (Date) values('"+newDaysString+"')";
                SqlCommand insertDay = new SqlCommand(queryInsertDay, connection6);
                insertDay.ExecuteNonQuery();
                connection6.Close();
                MessageBox.Show("The Date:" +newDaysString+ " is added to the timetable");
            }
        }
        // back button
        private void backBtn_Click(object sender, EventArgs e)
        {
            for (int i = 0; i < Controls.Count; i++)
            {
                Controls[i].Size = new Size(0,20);
            }
            sectionBtn.Size = new Size(134,51);
            doctorBtn.Size = new Size(134, 51);
            daysBtn.Size = new Size(134, 51);
        }

        
    }
    class SectionIdHolder
    {
        public static List<int> sectionIdList = new List<int>();
    }
    class DoctorIdHolder
    {
        public static List<int> doctorIdList = new List<int>();
    }
    class DaysIdHolder
    {
        public static List<int> daysIdList = new List<int>();
    }
    class QueueIdHolder
    {
        public static List<int> queueIdList = new List<int>();
    }

}



```
