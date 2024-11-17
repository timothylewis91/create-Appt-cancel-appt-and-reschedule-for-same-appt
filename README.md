# create-Appt-cancel-appt-and-reschedule-for-same-appt
Automation
package testScripts.execution;

import platformIndependentCore.scripts.Arguments;
import platformIndependentCore.scripts.TestScript;
import testScripts.Args;

/**
 * <b>ARGUMENTS: </b>
 * </p>
 * <ul>
 * <li><b>Args.YOUR_SCRIPT_ARG_1</b>= Short description of script argument</li>
 * <li><b>Args.YOUR_SCRIPT_ARG_2</b>= Short description of script argument</li>
 * </ul>
 * <p>
 * <b>RETURNS: </b>
 * </p>
 * <ul>
 * <li><b>ObjectType</b> - short description of returned object</li>
 * </ul>
 * <p>
 * <b>Script Name: </b> CreateScheduleCancelRescheduleAppt.java
 * <p>
 * <b>Description: </b>
 * <p>
 * <b>Pre-Condition: </b>
 * <p>
 * <b>Post-Condition: </b>
 *
 * @since Sep 19, 2024
 * @author OITSDCLEWIST
 */
public class CreateScheduleCancelRescheduleAppt extends TestScript {

	/**
	 * Main method. Will allow the user to run the script using the button in
	 * Eclipse
	 *
	 * @param args Args
	 */
	public static void main(String[] args) {
		runScript(args);

	}

	@Override
	public void testScript(Arguments args) {

		if (args.isEmpty()) {

			args.set(Args.PATIENT_SCRIPTS_DATA_FILE_NAME, "Patients.csv");
		}

		// Disable scanning for 508 compliance on pages, sub pages and modal forms
		args.set(Args.SCAN_PAGE_508, false);

		// *** Login ***
		// Login to the ISS home page
		// The runModularScript requires the path and name of the script along with any
		// arguments you want to pass
		runModularScript("testScripts.modular.Login", args);

		// *** Change Location ***
		// Run the Change Location script
		args.set(Args.PATIENT_NAME, "patientName1");
		runModularScript("testScripts.modular.ChangeLocation", args);

		// *** Search for Patient ***
		// Run the Search for Patient script
		runModularScript("testScripts.modular.SearchForPatient", args);

		// *** Go To Patient Page ***
		// The Page Name will be passed to the Go To Page script
		args.set(Args.PAGE_NAME, "PatientPage");
		runModularScript("testScripts.modular.GoToPage", args);
		runModularScript("testScripts.modular.CreateNewAppointmentRequestUniqueComment", args);

		// Set the Appointment Request filters
		// You only need to set the values for the particular columns you are filtering
		// on
		args.set(Args.FILTER_CLINIC_OR_SERVICE_VALUE, "AUTO BLACKBOX COUNT");
		args.set(Args.FILTER_COMMENT_VALUE, "NOTE-REPEAT-APPT");
		// args.set(Args.FILTER_PID_VALUE, "today"); // no leading zeros

		runModularScript("testScripts.modular.SetAppointmentRequestFilter", args);

		runModularScript("testScripts.modular.SelectNewAppointmentRequestUniqueComment", args);

		args.set(Args.APPT_CREATE, "Create Appointment");
		runModularScript("testScripts.modular.SelectTimeSlotCreateNewAppointmentUniqueComment", args);

		runModularScript("testScripts.modular.VerifyThenCreateOrCancelNewAppointment", args);

		// Cancel the APPT appointment for today
		args.set(Args.FILTER_CLINIC, "AUTO BLACKBOX COUNT");
		runModularScript("testScripts.modular.SetPendingAppointmentsFilter", args);
		args.set(Args.FILTER_DATE, "Sep 23, 2024@2:00 PM");
		args.set(Args.CANCELLATION_REASONS, "OTHER");
		runModularScript("testScripts.modular.CancelAppointmentFromPendingTable", args);
		// Verify appointment is cancelled
		args.set(Args.FILTER_CLINIC_OR_SERVICE_VALUE, "AUTO BLACKBOX COUNT");

		args.set(Args.FILTER_COMMENT_VALUE, "NOTE-REPEAT-APPT");

		runModularScript("testScripts.modular.VerifyAppointmentIsCancel", args);
		args.set(Args.FILTER_CLINIC_OR_SERVICE_VALUE, "AUTO BLACKBOX COUNT");
		args.set(Args.FILTER_COMMENT_VALUE, "NOTE-REPEAT-APPT");
		// args.set(Args.FILTER_PID_VALUE, "today"); // no leading zeros

		runModularScript("testScripts.modular.SetAppointmentRequestFilter", args);

		runModularScript("testScripts.modular.SelectNewAppointmentRequestUniqueComment", args);

		args.set(Args.APPT_CREATE, "Create Appointment");
		runModularScript("testScripts.modular.SelectTimeSlotCreateNewAppointmentUniqueComment", args);

		runModularScript("testScripts.modular.VerifyThenCreateOrCancelNewAppointment", args);

	}
}
