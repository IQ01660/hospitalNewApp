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
                var getSectionIdAdapter = new SqlDataAdapter(GetSectionId);
                var getSectionIdDataset = new DataSet();
                getSectionIdAdapter.Fill(getSectionIdDataset);
                string selectedSectionIdString = getSectionIdDataset.Tables[0].Rows[0]["Id"].ToString();
                int selectedSectionId = Convert.ToInt32(selectedSectionIdString);
                string queryStringDoctorInsert = "insert into [dbo].[Doctor] (Name,Surname,Section_Id) values('"+newDoctorName+"','"+newDoctorSurname+"',"+selectedSectionId+")";
                //adding the doctor to pivotal table
                //  finding the id of the doctor
                string queryStringGetDoctorId = "select Id from [dbo].[Doctor] where Name = '"+newDoctorName+"' and Surname = '"+newDoctorSurname+"'";
                SqlCommand GetDoctorId = new SqlCommand(queryStringGetDoctorId, connection);
                var getDoctorIdAdapter = new SqlDataAdapter(GetDoctorId);
                var getDoctorIdDataset = new DataSet();
                getDoctorIdAdapter.Fill(getDoctorIdDataset);
                string selectedDoctorIdString = getDoctorIdDataset.Tables[0].Rows[0]["Id"].ToString();
                int selectedDoctorId = Convert.ToInt32(selectedDoctorIdString); // the doctor id
                //  finding the id of the day
                string queryStringGetDaysId = "select Id from [dbo].[Days] where Date = '" + newDoctorDays + "'";
                SqlCommand GetDaysId = new SqlCommand(queryStringGetDaysId, connection);
                var getDaysIdAdapter = new SqlDataAdapter(GetDaysId);
                var getDaysIdDataset = new DataSet();
                getDaysIdAdapter.Fill(getDaysIdDataset);
                string selectedDaysIdString = getDaysIdDataset.Tables[0].Rows[0]["Id"].ToString();
                int selectedDaysId = Convert.ToInt32(selectedDaysIdString); // the days id
                //  finding the id of the queue
                string queryStringGetQueueId = "select Id from [dbo].[Queue] where Name = '" + newDoctorQueue + "'";
                SqlCommand GetQueueId = new SqlCommand(queryStringGetQueueId, connection);
                var getQueueIdAdapter = new SqlDataAdapter(GetQueueId);
                var getQueueIdDataset = new DataSet();
                getQueueIdAdapter.Fill(getQueueIdDataset);
                string selectedQueueIdString = getQueueIdDataset.Tables[0].Rows[0]["Id"].ToString();
                int selectedQueueId = Convert.ToInt32(selectedQueueIdString); // the days id
                //eventually inserting the data into the pivotal table
                string queryInsertIntoPivotal = 
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
}


```
