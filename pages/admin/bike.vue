<template>
	<v-container>
			<h3>Bike Management</h3>
            <br>
            <BikeForm />
	</v-container>
</template>

<script>
import BikeForm from '../../components/BikeForm'

	export default {
          components: {
             // Navbar,
            BikeForm
        },
		middleware: 'auth-admin'
	}
</script>