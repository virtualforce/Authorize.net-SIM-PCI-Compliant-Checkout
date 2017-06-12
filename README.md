#Authorize.net PCI Compliant Payment Gateway Integration

The Payment Card Industry Data Security Standard (PCI DSS) is a set of security standards designed to ensure the standard storage requirements is required,otherwise if you are not going to save any card related information to your own system you dont need to pass the PCI DSS standard.

Authorize.net provides SIM payment solution in which your server or code base doesn't need to be PCI Compliant as it redirects the customer directly to their own checkout page to capture card information.

First you just need to initialize the form object in your controller as:

@sim_transaction=AuthorizeNet::SIM::Transaction.new(login_id, transaction_id,@params[:amount], :hosted_payment_form => true,transaction_type:trans_type)
@sim_transaction.set_hosted_payment_receipt(AuthorizeNet::SIM::HostedReceiptPage.new(:link_method => AuthorizeNet::SIM::HostedReceiptPage::LinkMethod::GET, :link_text => 'Continue', :link_url => @params[:response_url]))

Second you just need to setup the form that has some fingerprint information to make it secure to carry out the necessary information. Form only has the checkout button visible and it will take the customer to authorize.net checkout payment page.

<%= form_for :sim_transaction, :url => AuthorizeNet::SIM::Transaction::Gateway::TEST,html:{class:"sim-transaction-form"} do |sim| %>
	<div class="sim-inputs">
	<%= sim_fields(@sim_transaction) %>
	</div>
	<INPUT TYPE=HIDDEN NAME="x_method" VALUE="CC">
	<INPUT TYPE=HIDDEN NAME="x_application_id" id= "app-id" value="<%=params[:application_id]%>">
	<INPUT TYPE=HIDDEN NAME="x_months" id= "p-months" value="<%= @patient_plan.new_record? ? 1 : @patient_plan.try(:no_of_months)%>">
	<INPUT TYPE=HIDDEN NAME="x_pp" id= "pp-edit" value="<%= @patient_plan.id %>">
	<INPUT TYPE=HIDDEN NAME="x_receipt_link_text"  value="Continue to Payment Plan Contract">
	<INPUT TYPE=HIDDEN NAME="x_is_dp" id='dp-check'>
	<INPUT TYPE=HIDDEN NAME="x_description" id='pay-desc'>
	<INPUT TYPE=HIDDEN NAME="x_st_dt" id='st-dt'>

	<%= sim.submit (t "patient_plans.submit"), class:"sim-submit sim-btns"%>
	<% end %>
